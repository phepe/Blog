---
title: Coco2dx游戏粒子系统反向查看
date: 2019-07-25 16:54:21
categories:
    - Game
tags:
    - Cocos2dx
---

### 1. 概述

有很多使用Cocos2dx 编写的游戏，粒子效果没有加密，直接是一个Plist的文件，因此我们可以反向查看学习参考。
<!-- more -->
### 2. Particle plist 文件分析

首先要确保plist文件的格式正确，网上有很多在线粒子编辑器，可以直接导入plist的 [编辑器](http://www.effecthub.com/particle2dx)  。

需要注意的主要有一下2点:

* plist文件的开头声明正确

      <?xml version="1.0" encoding="utf-8"?>
      <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
      <plist version="1.0">

*  图片名称`textureFileName`可以查看一下是否直接包含在apk里面，如果没有就查看 `textureImageData`数据是否有，如果通过在线工具查看效果，需要先把`textureImageData`数据删掉，因为无法正确反编码出图片。

* 通过`Cocos2d particle Editor`查看Plist会很方便，[下载地址](https://code.google.com/archive/p/cocos2d-windows-particle-editor/downloads)，注意项目分文源码和编译后的版本，不要下载错了，免安装的。

### 3.方向解码出PNG文件

使用`cocos2dx`工程可以反向解码出plist文件里面的图片，具体代码如下，主要是借鉴引擎里面的`CCParticleSystem.cpp`代码，使用的`cocos2d-x-3.17.2`创建了一个HelloWorld工程，调用以下函数就行，生成的图片在`C:\Users\hello\AppData\Local\GetParticlePNG`里面。

    void HelloWorld::base64ToImage()
    {
    	bool ret = false;
    	unsigned char *buffer = nullptr;
    	unsigned char *deflated = nullptr;
    	Image *image = nullptr;

    	std::string textureData = "base64Data";
    	CCASSERT(!textureData.empty(), "textureData can't be empty!");

    	auto dataLen = textureData.size();
    	if (dataLen != 0)
    	{
    		// if it fails, try to get it from the base64-gzipped data    
    		int decodeLen = base64Decode((unsigned char*)textureData.c_str(), (unsigned int)dataLen, &buffer);
    		CCASSERT(buffer != nullptr, "CCParticleSystem: error decoding textureImageData");

    		ssize_t deflatedLen = ZipUtils::inflateMemory(buffer, decodeLen, &deflated);
    		CCASSERT(deflated != nullptr, "CCParticleSystem: error ungzipping textureImageData");


    		// For android, we should retain it in VolatileTexture::addImage which invoked in Director::getInstance()->getTextureCache()->addUIImage()
    		image = new (std::nothrow) Image();
    		bool isOK = image->initWithImageData(deflated, deflatedLen);
    		CCASSERT(isOK, "CCParticleSystem: error init image with Data");
    		auto texture = Director::getInstance()->getTextureCache()->addImage(image, "test");
    		auto testSprite = Sprite::createWithTexture(texture);
    		// position the sprite on the center of the screen
    		testSprite->setPosition(Vec2(200, 200));
    		image->saveToFile(FileUtils::getInstance()->getWritablePath() + "test2.png", false);
    		this->addChild(testSprite, 0);
    		image->release();
    	}
    }

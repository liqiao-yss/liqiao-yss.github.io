---
layout: post
title: Java+Selenium+tess4j识别验证码
date: 2020-07-28
Author: mimo
categories: 
tags: [验证码,tess4j,识别]
comments: true
typora-root-url: ..\
---

### 导入selenium包

```java
<!--selenium 依赖--><dependency>
<groupId>org.seleniumhq.selenium</groupId>
<artifactId>selenium-java</artifactId>
<version>3.5.3</version></dependency>
```

### 识别方法：二值化+去噪

```java
/**
  * 去背景,去除干扰线
  * @param picFile
  * @return
  * @throws Exception
  */
public static BufferedImage removeBackgroud(String picFile) throws Exception {
    BufferedImage img = ImageIO.read(new File(picFile));
    int width = img.getWidth();
    int height = img.getHeight();
    for (int y = 1; y < height - 1; ++y) {
        for (int x = 1; x < width - 1; ++x) {
            //如果color[i+1][j],color[i-1][j],color[i][j+1],color[i][j-1]都是纯黑或者纯白色的，就认为color[i][j]是干扰，将color[i][j]置为白色。
            if (getColorBright(img.getRGB(x, y)) < 300) {
                if (isBlackOrWhite(img.getRGB(x - 1, y))
                    + isBlackOrWhite(img.getRGB(x + 1, y))
                    + isBlackOrWhite(img.getRGB(x, y - 1))
                    + isBlackOrWhite(img.getRGB(x, y + 1)) == 4) {
                    img.setRGB(x, y, Color.BLACK.getRGB());
                }
            }else {
                img.setRGB(x,y,Color.white.getRGB());
            }
        }
    }
    for (int x = 1; x < width - 1; ++x) {
        for (int y = 1; y < height - 1; ++y) {
            if (getColorBright(img.getRGB(x, y)) < 300) {
                if (isBlackOrWhite(img.getRGB(x - 1, y))
                    + isBlackOrWhite(img.getRGB(x + 1, y))
                    + isBlackOrWhite(img.getRGB(x, y - 1))
                    + isBlackOrWhite(img.getRGB(x, y + 1)) == 4) {
                    img.setRGB(x, y, Color.BLACK.getRGB());
                }
            }
        }
    }
    img = img.getSubimage(1, 1, img.getWidth() - 2, img.getHeight() - 2);
    return img;
}

public static int getColorBright(int colorInt) {
    Color color = new Color(colorInt);
    return color.getRed() + color.getGreen() + color.getBlue();

}

public static int isBlackOrWhite(int colorInt) {
    if (getColorBright(colorInt) < 300 || getColorBright(colorInt) > 500) {
        return 1;
    }
    return 0;
}
```

方法前：![](E:\blog\liqiao-yss.github.io\document_image\3.png)

方法后：![](E:\blog\liqiao-yss.github.io\document_image\3plus.png)

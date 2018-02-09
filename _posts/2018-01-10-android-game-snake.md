---
author: zhangman1123
comments: true
date: 2018-01-02 09:51:14+00:00
layout: post
slug: Android-Snake-Game
title: 手把手教你写Android 贪吃蛇 游戏
tags:
- Android
categories:
- Android Game
---

## 手把手教你写Android 贪吃蛇 游戏

先看看效果图

![]({{ site.url }}/assets/images/game-snake.gif)

### 贪吃蛇设计思路

贪吃蛇分为3个对象：

* 蛇

* 食物

* 舞台

舞台我们可以看作为一个二维数组  蛇和食物 都是数组中的元素 

蛇是一串数组中的连续的元素  分为蛇的头元素和蛇身长度

食物可以看作是数组中的一个元素 

#### 蛇的移动

蛇可以向上，向下，向左，向右移动

蛇移动 头元素+1 尾元素-1

#### 碰撞检测

当蛇的头部元素碰撞到食物 则吃掉食物 蛇长度+1。 如果碰撞到蛇身 游戏结束，到舞台边界 直接穿过去

#### 随机生成食物 

使用`Random` 生成食物坐标 x，和y 生成后判断是不是在蛇身坐标上，如果是重新生成。

### 代码实现
首先我们使用自定义`View` 来实现
{% highlight java %}
public class SnakePanelView extends View {
  
  @Override
  protected void onDraw(Canvas canvas) {
    super.onDraw(canvas);
    //绘制界面...
  }
}
{% endhighlight %}

然后我们定义格子元素

{% highlight java %}
public class GridSquare {
  private int mType;//元素类型

  public GridSquare(int type) {
    mType = type;
  }

  public int getColor() {
    switch (mType) {
      case GameType.GRID://空格子
        return Color.WHITE;
      case GameType.FOOD://食物
        return Color.BLUE;
      case GameType.SNAKE://蛇
        return Color.parseColor("#FF4081");
    }
    return Color.WHITE;
  }

  public void setType(int type) {
    mType = type;
  }
}
{% endhighlight %}

这个就是格子对象

然后我们定义出的二维数组舞台定义如下:
{% highlight java %}
private List<List<GridSquare>> mGridSquare = new ArrayList<>();
{% endhighlight %}

定义出了舞台数组 我们需要定义食物的位置和蛇的位置。所以我们需要定义一个对象 
这个对象需要两个成员变量 `x` 和 `y` 来标示在舞台中的位置
{% highlight java %}
public class GridPosition {
  private int x;
  private int y;

  public GridPosition(int x, int y) {
    this.x = x;
    this.y = y;
  }

  public int getX() {
    return x;
  }

  public void setX(int x) {
    this.x = x;
  }

  public int getY() {
    return y;
  }

  public void setY(int y) {
    this.y = y;
  }
}
{% endhighlight %}

然后我们需要定义蛇头位置、蛇身`List` 和食物的位置
{% highlight java %}
  private List<GridPosition> mSnakePositions = new ArrayList<>();
  private GridPosition mSnakeHeader;//蛇头部位置
  private GridPosition mFoodPosition;//食物的位置
{% endhighlight %}

然后我们需要定义蛇的一系列属性：速度、蛇身长度和运动方向。

{% highlight java %}
 private int mSnakeLength = 3;//蛇身长度
 private long mSpeed = 8;//运行速度 值越大 速度越快
 private int mSnakeDirection = GameType.RIGHT;//运行方向 
{% endhighlight %}

让蛇运动起来我们需要一个游戏主循环
我们可以简单定义一个循环
{% highlight java %}
private class GameMainThread extends Thread {

    @Override
    public void run() {
      while (true) {
        //todo...
      }
    }
}
{% endhighlight %}

我们在循环里处理蛇移动、碰撞检测和食物生产等操作。

处理蛇的移动方法:
{% highlight java %}
private void moveSnake(int snakeDirection) {
    switch (snakeDirection) {
      case GameType.LEFT:
        if (mSnakeHeader.getX() - 1 < 0) {//边界判断：如果到了最左边 让他穿过屏幕到最右边 mGridSize 为舞台大小
          mSnakeHeader.setX(mGridSize - 1);
        } else {
          mSnakeHeader.setX(mSnakeHeader.getX() - 1);
        }
        mSnakePositions.add(new GridPosition(mSnakeHeader.getX(), mSnakeHeader.getY()));
        break;
      case GameType.TOP:
        if (mSnakeHeader.getY() - 1 < 0) {
          mSnakeHeader.setY(mGridSize - 1);
        } else {
          mSnakeHeader.setY(mSnakeHeader.getY() - 1);
        }
        mSnakePositions.add(new GridPosition(mSnakeHeader.getX(), mSnakeHeader.getY()));
        break;
      case GameType.RIGHT:
        if (mSnakeHeader.getX() + 1 >= mGridSize) {
          mSnakeHeader.setX(0);
        } else {
          mSnakeHeader.setX(mSnakeHeader.getX() + 1);
        }
        mSnakePositions.add(new GridPosition(mSnakeHeader.getX(), mSnakeHeader.getY()));
        break;
      case GameType.BOTTOM:
        if (mSnakeHeader.getY() + 1 >= mGridSize) {
          mSnakeHeader.setY(0);
        } else {
          mSnakeHeader.setY(mSnakeHeader.getY() + 1);
        }
        mSnakePositions.add(new GridPosition(mSnakeHeader.getX(), mSnakeHeader.getY()));
        break;
    }
}
{% endhighlight %}

处理碰撞:
{% highlight java %}
//检测碰撞
private void checkCollision() {
    //检测是否咬到自己
    GridPosition headerPosition = mSnakePositions.get(mSnakePositions.size() - 1);
    for (int i = 0; i < mSnakePositions.size() - 2; i++) {
      GridPosition position = mSnakePositions.get(i);
      if (headerPosition.getX() == position.getX() && headerPosition.getY() == position.getY()) {
        //咬到自己 停止游戏
        mIsEndGame = true;
        showMessageDialog();
        return;
      }
    }

    //判断是否吃到食物
    if (headerPosition.getX() == mFoodPosition.getX()
        && headerPosition.getY() == mFoodPosition.getY()) {
      mSnakeLength++;
      generateFood();
    }
}
{% endhighlight %}

随机生成食物：
{% highlight java %}
private void generateFood() {
    Random random = new Random();
    int foodX = random.nextInt(mGridSize - 1);
    int foodY = random.nextInt(mGridSize - 1);
    for (int i = 0; i < mSnakePositions.size() - 1; i++) {
      if (foodX == mSnakePositions.get(i).getX() && foodY == mSnakePositions.get(i).getY()) {
        //不能生成在蛇身上
        foodX = random.nextInt(mGridSize - 1);
        foodY = random.nextInt(mGridSize - 1);
        //重新循环
        i = 0;
      }
    }
    mFoodPosition.setX(foodX);
    mFoodPosition.setY(foodY);
    refreshFood(mFoodPosition);
}
{% endhighlight %}

基本的游戏逻辑已经完成。

工程已经放在[GITHUb](https://github.com/zhangman523/AndroidGameSnake)

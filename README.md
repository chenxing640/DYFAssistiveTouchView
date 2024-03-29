[如果此项目能帮助到你，就请你给一颗星。谢谢！(If this project can help you, please give it a star. Thanks!)](https://github.com/chenxing640/DYFAssistiveTouchView)

[![License MIT](https://img.shields.io/badge/license-MIT-green.svg?style=flat)](LICENSE)&nbsp;
[![CocoaPods](http://img.shields.io/cocoapods/v/DYFAssistiveTouchView.svg?style=flat)](http://cocoapods.org/pods/DYFAssistiveTouchView)&nbsp;
![CocoaPods](http://img.shields.io/cocoapods/p/DYFAssistiveTouchView.svg?style=flat)&nbsp;

## DYFAssistiveTouchView

实现应用内悬浮按钮和辅助工具条，可以增加/修改 Item 项，通过事件索引，完成各种场景页面的跳转。

## Installation

Using [CocoaPods](https://cocoapods.org):

```
 pod 'DYFAssistiveTouchView', '~> 4.2.3'
```

Or

```
# Install lastest version.
pod 'DYFAssistiveTouchView'
```

## Group (ID:614799921)

<div align=left>
&emsp; <img src="https://github.com/chenxing640/DYFAssistiveTouchView/raw/master/images/g614799921.jpg" width="30%" />
</div>

## Priview

<div align=left>
&emsp; <img src="https://github.com/chenxing640/DYFAssistiveTouchView/raw/master/images/AssistiveTouchViewPreview.gif" width="30%" />
</div>

## Usage

1. 实例化

```ObjC
// Lazy load
- (DYFAssistiveTouchView *)touchView {
    if (!_touchView) {
        _touchView = [[DYFAssistiveTouchView alloc] init];
        _touchView.frame = CGRectMake(0, 0, 50, 50);
    }
    return _touchView;
}
```

2. 设置属性

```ObjC
// 设置按钮的各种状态的图像
UIImage        *leftHidenImage = [UIImage imageNamed:@"atv_hide_left"];
UIImage       *rightHidenImage = leftHidenImage;
UIImage       *leftNormalImage = [UIImage imageNamed:@"atv_normal_left"];
UIImage      *rightNormalImage = leftNormalImage;
UIImage  *leftHighlightedImage = [UIImage imageNamed:@"atv_normal_left"];
UIImage *rightHighlightedImage = leftHighlightedImage;

self.touchView.touchObject.leftNormalImage       = leftNormalImage;
self.touchView.touchObject.rightNormalImage      = rightNormalImage;
self.touchView.touchObject.leftHighlightedImage  = leftHighlightedImage;
self.touchView.touchObject.rightHighlightedImage = rightHighlightedImage;
self.touchView.touchObject.leftTranslucentImage  = leftHidenImage;
self.touchView.touchObject.rightTranslucentImage = rightHidenImage;

// 设置组件对象
UIImage  *leftUint1Image = [UIImage imageNamed:@"atv_unit1_left"];
UIImage *rightUint1Image = [UIImage imageNamed:@"atv_unit1_right"];
UIImage  *leftUint2Image = [UIImage imageNamed:@"atv_unit2_left"];
UIImage *rightUint2Image = [UIImage imageNamed:@"atv_unit2_right"];

self.touchView.unitObject.leftTouchImage           = leftUint1Image;
self.touchView.unitObject.rightTouchImage          = rightUint1Image;
self.touchView.unitObject.leftItemBackgroundImage  = leftUint2Image;
self.touchView.unitObject.rightItemBackgroundImage = rightUint2Image;

// 设置item对象
UIImage *userImage = [UIImage imageNamed:@"atv_item_user"];
UIImage *cafeImage = [UIImage imageNamed:@"atv_item_cafe"];
UIImage   *csImage = [UIImage imageNamed:@"atv_item_cs"];

DYFAssistiveTouchItem *item  = [[DYFAssistiveTouchItem alloc] init];
item.image  = userImage;
DYFAssistiveTouchItem *item1 = [[DYFAssistiveTouchItem alloc] init];
item1.image = cafeImage;
DYFAssistiveTouchItem *item2 = [[DYFAssistiveTouchItem alloc] init];
item2.image = csImage;

self.touchView.items = @[item, item1, item2];
```

3. 是否显示

```ObjC
[self.touchView isShowing]
```

4. 显示

```ObjC
[self.touchView show];
```

5. 隐藏

```ObjC
[self.touchView hide];
```

6. 隐藏一半至屏幕

```ObjC
[self.touchView setShouldShowHalf:YES];
```

7. 设置初始显示位置

```ObjC
[self.touchView setTouchViewPlace:DYFTouchViewAtMiddleRight];
```

8. 响应事件(二选一)

 - block实现
 
```ObjC
- (IBAction)configureAction:(id)sender {
    if (!_touchView) {
        [self setImagesForTouchView];
        [self setUnitsForTouchView];
        [self setItemsForTouchView];

        [self.touchView setShouldShowHalf:YES];
        [self.touchView setTouchViewPlace:DYFTouchViewAtMiddleRight];

        __weak typeof(self) weakSelf = self;
        [self.touchView touchViewItemDidClickedAtIndex:^(DYFAssistiveTouchView *touchView) {
            NSInteger index = touchView.indexOfItem;
            NSLog(@"Index of item: %zi", index);
            [weakSelf presentAtIndex:index];
        }];
    }
}

- (void)presentAtIndex:(NSInteger)index {
    NSString *url = @"https://support.apple.com/zh-cn";

    if (index == 0) {
        url = @"https://github.com/chenxing640";
    } 
    else if (index == 1) {
        url = @"https://github.com/chenxing640/OpenSource";
    } 
    else {
        url = @"https://www.jianshu.com/u/7fc76f1179cc";
    }

    SFSafariViewController *safariVC = [[SFSafariViewController alloc] initWithURL:[NSURL URLWithString:url]];
    [self presentViewController:safariVC animated:YES completion:NULL];
}
```

- 代理实现

```ObjC
// 协议
Protocol: <DYFAssistiveTouchViewDelegate>

// 设置代理 
self.touchView.delegate = self;

// 代理实现
- (void)touchViewItemDidClickedAtIndex:(DYFAssistiveTouchView *)touchView {
    NSInteger index = touchView.indexOfItem;
    NSLog(@"Index of item: %zi", index);
    [self presentAtIndex:index]; 
}

- (void)presentAtIndex:(NSInteger)index {
    NSString *url = @"https://support.apple.com/zh-cn";

    if (index == 0) {
        url = @"https://github.com/chenxing640";
    } 
    else if (index == 1) {
        url = @"https://github.com/chenxing640/OpenSource";
    } 
    else {
        url = @"https://www.jianshu.com/u/7fc76f1179cc";
    }

    SFSafariViewController *safariVC = [[SFSafariViewController alloc] initWithURL:[NSURL URLWithString:url]];
    [self presentViewController:safariVC animated:YES completion:NULL];
}
```

## Code Sample

- [Code Sample Portal](https://github.com/chenxing640/DYFAssistiveTouchView/blob/master/Basic%20Files/ViewController.m)

![LTInfiniteScrollView](https://cocoapod-badges.herokuapp.com/v/LTInfiniteScrollView/badge.png)

## Demo
![LTInfiniteScrollView](https://raw.githubusercontent.com/ltebean/LTInfiniteScrollView/master/demo.gif)

## Usage

Create the scroll view by:
```objective-c
self.scrollView = [[LTInfiniteScrollView alloc]initWithFrame:CGRectMake(0, 0, CGRectGetWidth(self.view.bounds), 200)];
[self.view addSubview:self.scrollView];
self.scrollView.dataSource = self;
[self.scrollView reloadData];
```

Then implement `LTInfiniteScrollViewDataSource` protocol:
```objective-c
@protocol LTInfiniteScrollViewDataSource <NSObject>
- (UIView *)viewAtIndex:(NSInteger)index reusingView:(UIView *)view;
- (NSInteger)numberOfViews;
- (NSInteger)numberOfVisibleViews;
@end
```

Sample code:
```objective-c
- (NSInteger)totalViewCount
{
    // you can set it to a very big number to mimic the infinite behavior, no performance issue here
    return 9999; 
}

- (NSInteger)visibleViewCount
{
    return 5;
}

- (UIView *)viewAtIndex:(NSInteger)index reusingView:(UIView *)view;
{
    if (view) {
        ((UILabel*)view).text = [NSString stringWithFormat:@"%ld", index];
        return view;
    }
    
    UILabel *aView = [[UILabel alloc]initWithFrame:CGRectMake(0, 0, 64, 64)];
    aView.backgroundColor = [UIColor blackColor];
    aView.layer.cornerRadius = 32;
    aView.layer.masksToBounds = YES;
    aView.backgroundColor = [UIColor colorWithRed:0/255.0 green:175/255.0 blue:240/255.0 alpha:1];
    aView.textColor = [UIColor whiteColor];
    aView.textAlignment = NSTextAlignmentCenter;
    aView.text = [NSString stringWithFormat:@"%ld", index];
    return aView;
}
```

`LTInfinitedScrollView` interface:
```objective-c
@interface LTInfiniteScrollView : UIView
@property (nonatomic) NSInteger currentIndex;
@property (nonatomic,weak) id<LTInfiniteScrollViewDataSource> dataSource;
@property (nonatomic,weak) id<LTInfiniteScrollViewDelegate> delegate;
@property (nonatomic) BOOL scrollEnabled;
@property (nonatomic) BOOL pagingEnabled;
@property (nonatomic) NSInteger maxScrollDistance; 

- (void)reloadData;
- (void)scrollToIndex:(NSInteger)index animated:(BOOL)animated;
- (UIView *)viewAtIndex:(NSInteger)index;
- (NSArray *)allViews;
@end
```

If you want to apply any animation during scrolling, implement `LTInfiniteScrollViewDelegate` protocol: 
```objective-c
@protocol LTInfiniteScrollViewDelegate <NSObject>
- (void)updateView:(UIView *)view withDistanceToCenter:(CGFloat)distance scrollDirection:(ScrollDirection)direction;
@end
```
See the example for details~ 
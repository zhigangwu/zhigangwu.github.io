---

title: MapKit长按定位

date: 2019-3-16

---

项目中填了一个新功能大致是输入地址定位和长按定位，在此做个总结来赠强对知识点的记忆。
先上效果图

![effect]()


* 关键知识点

1.CLGeocoder地理编码和反地理编码:

```
//地理编码
- (void)geocodeAddressString:(NSString *)addressString completionHandler:(CLGeocodeCompletionHandler)completionHandler;
```

> 通过addressString:(定位的地址)获取到经纬度,在通过CLGeocoder的反地理编码定位

```
// 反地理编码
- (void)reverseGeocodeLocation:(CLLocation *)location completionHandler:(CLGeocodeCompletionHandler)completionHandler;
```

2.长按定位使用了:UILongPressGestureRecognizer长按手势

```
    UILongPressGestureRecognizer *longPress = [[UILongPressGestureRecognizer alloc] initWithTarget:self action:@selector(longPress:)]; // 长按手势
    longPress.delegate = self; // 记得添加代理UIGestureRecognizerDelegate
    [_mapView addGestureRecognizer:longPress];
```

长按手势监听

* 把长按的点位转成经纬度

```
    CGPoint touchPoint = [gestureRecognizer locationInView:_mapView];
    CLLocationCoordinate2D touchMapCoordinate = [_mapView convertPoint:touchPoint toCoordinateFromView:_mapView];
    
    [self setLocationWithLatitude:touchMapCoordinate.latitude AndLongitude:touchMapCoordinate.longitude]; // 自定义的一个方法主要功能是反地理编码

```

* 实现反地理编码方法

```
- (void)setLocationWithLatitude:(CLLocationDegrees)latitude AndLongitude:(CLLocationDegrees)longitude;
```

> 长按定位因为只需要当前的最新定位因此在长按手势监听中需要删除之前定位的大头针图标

```
    if (_mapView.annotations.count > 0) {
        NSMutableArray *removeAnnotations = [[NSMutableArray alloc]init];
        //将所有需要移除打大头针添加一个数组，去掉当前位置的大头针
        [removeAnnotations addObjectsFromArray:self.mapView.annotations];
        [removeAnnotations removeObject:self.mapView.userLocation];
        //移除需要移除的大头针
        [self.mapView removeAnnotations:removeAnnotations];
    }

``` 

* 个人知识整理不喜勿喷，如有差错不领赐教！！！！！

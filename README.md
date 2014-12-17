h5-
===yi
h5之移动端坑点

坑点1.
 问题：在移动端使用弹出层时, 弹窗滚动，带动父窗滚动条一起滚动。

尝试：
判断使用touchmove是否超出弹窗移动范围，添加e.preventDefault()， 禁用默认  事件； 

                 var pointerEventToXY = function(e){
                  var out = {x:0, y:0};
                  if(e.type == 'touchstart' || e.type == 'touchmove' || e.type == 'touchend' || e.type == 'touchcancel'){
                    var touch = e.touches[0] || e.changedTouches[0];
                    out.x = touch.pageX;
                    out.y = touch.pageY;
                  } else if (e.type == 'mousedown' || e.type == 'mouseup' || e.type == 'mousemove' || e.type == 'mouseover'|| e.type=='mouseout' || e.type=='mouseenter' || e.type=='mouseleave') {
                    out.x = e.pageX;
                    out.y = e.pageY;
                  }
                  return out;
                };
                    el.addEventListener('touchstart', function (e) {
                              sy = pointerEventToXY(e).y;
                              console.log(sy)
                   });

                    el.addEventListener('touchmove', function (e) {
                        

                         console.log("y:"+pointerEventToXY(e).y)
                         console.log("sy:"+sy)
                          var down = (pointerEventToXY(e).y - sy > 0);
                          

                          //top
                          if (down && el.scrollTop <= 0) {
                            e.preventDefault();
                          }
                          //bottom
                          if (!down && el.scrollTop >= el.scrollHeight - el.clientHeight) {
                            e.preventDefault();
                          }


                         
                    }, false);
    
 尝试使用后, 触摸移动点可以禁用了，但是如果直接一个点滑动很长距离，依然是有问题的。                   
 所以最后
 解决办法 ：
 默认样式
 html，body｛
     overflow：auto；//避免首次弹窗跳动
     height:100%; //  使webkit 内核浏览器 生效
 ｝
 触发弹窗时，使overflow纵截断
 html，body｛
     overflow-y:hidden；
  ｝
弹窗消失时，使overflow恢复默认；
html，body｛
     overflow-y:auto；
  ｝
弹窗

 

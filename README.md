# atob_animation

A to B Flutter package Animation

<img src="https://raw.githubusercontent.com/kakzaki/atob_animation/master/gif/untitled.gif" width="450px">


## 🎖 Installing

dependencies:
  atob_animation: any

### ⚡️Import


import 'package:atob_animation/atob_animation.dart';


#### ❤️How to Use

    import 'package:atob_animation/atob_animation.dart';
    import 'package:flutter/material.dart';
    
    void main() {
      runApp(MyApp());
    }
    
    class MyApp extends StatelessWidget {
      // This widget is the root of your application.
      @override
      Widget build(BuildContext context) {
        return MaterialApp(
          title: 'Flutter Demo',
          theme: ThemeData(
            primarySwatch: Colors.blue,
          ),
          home: MyHomePage(title: 'Flutter Demo Home Page'),
        );
      }
    }
    
    class MyHomePage extends StatefulWidget {
      MyHomePage({Key key, this.title}) : super(key: key);
    
      final String title;
    
      @override
      _MyHomePageState createState() => _MyHomePageState();
    }
    
    class _MyHomePageState extends State<MyHomePage> {
      int _counter = 0;
    
      GlobalKey _targetkey = GlobalKey();
      Offset _endOffset;
    
      GlobalKey _startkey = GlobalKey();
      Offset _startOffset;
    
      _incrementCart(){
        var _overlayEntry = OverlayEntry(builder: (_) {
          return AtoBAnimation(
            startPosition: _startOffset, //animation Start Point
            endPosition: _endOffset, //Animation end Point
            dxCurveAnimation: 100,
            dyCurveAnimation: 50,
            duration: Duration(milliseconds: 1000),
            opacity: 0.9,
            child: ClipOval(child: Container(
              color: Colors.orange[700],
              child: Padding(
                padding: const EdgeInsets.all(12.0),
                child: Icon(Icons.shopping_cart,color: Colors.white,size: 40,),
              ),
            )),
          );
        });
        // Show Overlay
        Overlay.of(context).insert(_overlayEntry);
        // wait for the animation to end
        Future.delayed(Duration(milliseconds: 2000), () {
          _overlayEntry.remove();
          _overlayEntry = null;
        });
    
        setState(() {
          _counter++;
        });
      }
    
    
      @override
      void initState() {
        super.initState();
        WidgetsBinding.instance.addPostFrameCallback((c) {
          // Get the location of the "shopping cart"
          _endOffset = (_targetkey.currentContext.findRenderObject() as RenderBox).localToGlobal(Offset.zero);
          // Get the position of the current widget when clicked, and pass in overlayEntry
          _startOffset = (_startkey.currentContext.findRenderObject() as RenderBox).localToGlobal(Offset.zero);
        });
      }
    
      @override
      Widget build(BuildContext context) {
        return Scaffold(
          appBar: AppBar(
            title: Text(widget.title),),
          body: Column(
            crossAxisAlignment: CrossAxisAlignment.center,
            mainAxisAlignment: MainAxisAlignment.start,
            children: [
              SizedBox(height: 100,),
              Center(
                child: Container(
                  key: _targetkey,
                  child: _counter != 0?
                  Stack(
                    children: [
                      Icon(Icons.shopping_bag, size: 100),
                      ClipOval(
                        child: Container(
                          color: Colors.yellow,
                          child: Padding(
                            padding: const EdgeInsets.all(4.0),
                            child: Text("$_counter",style: TextStyle(color: Colors.red,fontSize: 35),),
                          ),
                        ),
                      )
                    ],
                  ):
                  Icon(Icons.shopping_bag, size: 100),
                ),
              )
            ],
          ),
          floatingActionButton: FloatingActionButton(
            key: _startkey,
            onPressed:_incrementCart,
            child: Icon(Icons.add),
          ),
        );
      }
    }


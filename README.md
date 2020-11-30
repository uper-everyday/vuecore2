# vuecore2
vue2.0核心原理

1.index.html
#vue和模板
（1）编写页面模板
    1> 直接在html页面中写script标签
    2> 使用template
    3> 使用单文件</template>
 (2) 编写vue实例
    1.在vue的构造函数中提供data，compputed，methods等等
（3） 将vue实例挂载到html页面中（mount）


2.数据驱动模型
vue的执行流程
1.获取模板，模板中有“坑”
2.利用模板函数中的data来填坑，得到可以在页面中显示的标签
3，将标签替换原来有坑的标签

// 要解决一个问题:
// 使用 'xxx.yyy.zzz' 可以来访问某一个对象
// 就是用字符串路径来访问对象的成员
function getValueByPath( obj, path ) {
    let paths = path.split( '.' ); // [ xxx, yyy, zzz ]
    // 先 取得 obj.xxx, 再取得 结果中的 yyy, 再取得 结果中的 zzz
    // let res = null;
    // res = obj[ paths[ 0 ] ];
    // res = res[ paths[ 1 ] ];
    // res = res[ paths[ 2 ] ];

    let res = obj;
    let prop;
    while( prop = paths.shift() ) {
    res = res[ prop ];
    }
    return res;
}
 1》模板不变，数据在不断的变化，vue技巧性的利用函数柯里化 ，减少函数调用，提高性能
 2》vue编译的时候可以将模板转化为抽象语法树-》虚拟DOM-》渲染页面

 // 这个函数是在 Vue 编译 模板的时候就生成了
    function createGetValueByPath( path ) {
      let paths = path.split( '.' ); // [ xxx, yyy, zzz ]
      
      return function getValueByPath( obj ) {
        let res = obj;
        let prop;
        while( prop = paths.shift() ) {
          res = res[ prop ];
        }
        return res;
      }
    }
    let getValueByPath = createGetValueByPath( 'a.b.c.d' );

    var o = {
      a: {
        b: {
          c: {
            d: {
              e: '正确了'
            }
          }
        }
      }
    };

    var res = getValueByPath( o );
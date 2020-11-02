# 技术业务交流分享会
## 1. Optional 类
> Java 8 引入用于解决臭名昭著的空指针问题(NullPointerException)

样例代码：
```
    // 引发空指针问题
    FcjyBaxxDTO fcjyClfHtxx = null;
    String xmid = fcjyClfHtxx.getBdcSlXmDO().getXmid();

    // 避免空指针问题的判空
    if(null != fcjyClfHtxx){
        if(null != fcjyClfHtxx.getBdcSlXmDO()){
            xmid = fcjyClfHtxx.getBdcSlXmDO().getXmid();
        }
    }
```
使用 Optional 类来解决
```
    String xmid = "";
    FcjyBaxxDTO fcjyClfHtxx = null;
    Optional<BdcSlXmDO> optional = Optional.ofNullable(fcjyClfHtxx.getBdcSlXmDO());

    // 使用 isPresent 检查对象 false(null) true(不为空)
    if(optional.isPresent()){
        xmid = optional.get().getXmid();
    }
```
采用链式写法，并返回默认值，让你的代码更简洁
```
    FcjyBaxxDTO fcjyClfHtxx = null;

    String xmid = Optional.ofNullable(fcjyClfHtxx)
                .flatMap(t->Optional.ofNullable(t.getBdcSlXmDO()))
                .flatMap(t->Optional.ofNullable(t.getXmid())).orElse("default");
       
    System.out.println(xmid);  // default
```

总结：采用 Optional 在一定程度上能够减少代码的行数，使代码看起来更加的清晰，也可以减少一些不必要的空指针问题。大家可以视情况使用。

## 2. Builder 模式
> 大篇幅 Set 方法的终结者

样例代码：
```
    // 初始化收件人信息
    BdcSlYjxxDO bdcSlYjxxDO = new BdcSlYjxxDO();
    bdcSlYjxxDO.setSlbh(bdcXmDTOList.get(0).getSlbh());
    bdcSlYjxxDO.setGzlslid(gzlslid);
    bdcSlYjxxDO.setSjrxxdz(bdcQlrDO.getQlrmc());
    bdcSlYjxxDO.setSjrlxdh(bdcQlrDO.getDh());
    bdcSlYjxxDO.setSjryb(bdcQlrDO.getYb());
    bdcSlYjxxDO.setSjrxxdz(bdcQlrDO.getTxdz());
```
大量的 Set 代码，写起来费时费劲。 使用构造方法？ 
```
    BdcSlYjxxDO bdcSlYjxxDO = new BdcSlYjxxDO(slbh, gzlslid, qlrmc, dh, yb, txdz);
```
使用 Builder 模式来解决：
```
public class BdcSlYjxxDO {
    private String name;
    private Stirng yb;
    private String dh;
    // ...get ...set

    public static class Builder{
       private String name;
       private Stirng yb;
       private String dh;

       public Builder(){}
        
       public Builder(String name){ 
           this.name = name;
           return this;
       }

        public Builder(String yb){ 
           this.yb = yb;
           return this;
        }

        public BdcSlYjxxDO build(){
            return new BdcSlYjxxDO(this);
        }
    }

    private BdcSlYjxxDO(Builder builder){
        this.name = builder.name;
        this,yb = builder.yb;
        this.dh = builder.dh;
    }
}

    // builder 的使用
    BdcSlYjxxDO bdcSlYjxxDO = new BdcSlYjxxDO.Builder()
                .name("张三").yb("200000").dh("13900000000").build();
```
譬如很多业务情况下，类中的属性值可能都有值了，而我们只需要对类中的某几个属性进行赋值时，除了 Set 还可以采用以下方法
```
   // 在 BdcSlYjxxDO 类中新增方法设置属性值并返回 BdcSlYjxxDO
    public BdcSlYjxxDO withGzlslid(String gzlslid){
        this.setGzlslid(gzlslid);
        return this;
    }
    
    public BdcSlYjxxDO withSlbh(String slbh)｛
        this.setSlbh(slbh);
        return this;
    ｝

    // 使用方法
    bdcSlYjxxDO.withGzlslid("111").withSlbh("222");
```
是不是显得代码简洁了很多，高大上了很多。哈哈哈.....


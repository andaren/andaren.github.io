# 服务端返回码设计

## 异常设计
### 异常种类
- ServiceInvalidParam  
用于参数校验或者传输异常
- ServiceException  
用于业务处理步骤中的异常
- ServiceError  
用于业务处理过程中，程序出现了不可恢复的error导致的错误

### 异常统一处理
在最外层拦截异常处理后包装成前端友好展示的错误信息(暴露code)
## 通用业务异常码
### 业务异常码
```java
public enum CommonServiceCode implements ServiceCode {
    // 普通业务异常
    COMMON_BIZ_EXP("301"),
    // 三方系统调用异常
    THIRD_PART_SERVICE_EXP("404");





    // 异常码前缀
    private static final String codePrefix = "SI_";
    private String code;

    private String messge;

    CommonServiceCode(String code) {
        this.code = codePrefix + code;
        this.messge = "";
    }

    /**
     * 根据异常信息，生成异常Code
     * @param message
     * @return
     */
    public ServiceCode ofMsg(String message) {
        String thisCode = this.code();
        return new ServiceCode() {
            @Override
            public String code() {
                return thisCode;
            }

            @Override
            public String message() {
                return message;
            }
        };
    }

    @Override
    public String code() {
        return this.code;
    }

    @Override
    public String message() {
        return this.messge;
    }
}
```
### 使用
> throw new ServiceException(CommonServiceCode.COMMON_BIZ_EXP.ofMsg("调用第三方服务异常：返回值空"));
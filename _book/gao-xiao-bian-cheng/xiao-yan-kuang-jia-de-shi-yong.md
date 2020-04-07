# 校验框架的使用

* 除了常用的简单的校验之外，特别注意的是**级联校验**和**分组校验**
* 分组校验用于校验适用在需要哪些场景校验，哪些场景不需要校验，而分组校验还有可能多个组别，顺序可能和原先的不一致，可以使用一个@groupSequenceValidation的注解（可以上网查看怎么使用）
* 参数校验
* 返回值校验
* 构造方法校验
* 一个自定义的手机验证器的例子

```java

/***
 * 自定义手机号约束注解关联验证器
 */
public class PhoneValidator implements ConstraintValidator<Phone,String> {
    @Override
    public boolean isValid(String s, ConstraintValidatorContext constraintValidatorContext) {

        //随便一个手机号的验证规则：
        String check  = "158\\d{8}";
        Pattern regex = Pattern.compile(check);
       //传入的值为空的处理
        String s1 = Optional.ofNullable(s).orElse("");
        Matcher matcher = regex.matcher(s1);
        return matcher.matches();
    }
}

//写的注解
package com.agth;

import javax.validation.Constraint;
import javax.validation.Payload;
import java.lang.annotation.*;

@Documented
//注解的作用目标
@Target({ElementType.FIELD})
//注解的保留策略
@Retention(RetentionPolicy.RUNTIME)
//和约束注解关联的验证器
@Constraint(validatedBy = PhoneValidator.class)
public @interface Phone {
    //约束注解验证输出的信息
    String message() default "手机号验证有误";

    //约束注解在验证时所属的组别
    Class<?>[] groups() default {};

    //约束注解的有效负载
    Class<? extends Payload>[] payload() default {};
}

```




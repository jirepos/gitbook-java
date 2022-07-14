# Java  주석
```

@author (classes and interfaces only, required)
@version (classes and interfaces only, required. See footnote 1)
@param (methods and constructors only)
@return (methods only)
@exception (@throws is a synonym added in Javadoc 1.2)
@see
@since
@serial (or @serialField or @serialData)
@deprecated (see How and When To Deprecate APIs)


@see #field
@see #Constructor(Type, Type...)
@see #Constructor(Type id, Type id...)
@see #method(Type, Type,...)
@see #method(Type id, Type, id...)
@see Class
@see Class#field
@see Class#Constructor(Type, Type...)
@see Class#Constructor(Type id, Type id)
@see Class#method(Type, Type,...)
@see Class#method(Type id, Type id,...)
@see package.Class
@see package.Class#field
@see package.Class#Constructor(Type, Type...)
@see package.Class#Constructor(Type id, Type id)
@see package.Class#method(Type, Type,...)
@see package.Class#method(Type id, Type, id)
@see package


/**
 * This is an "at" symbol: {@literal @}
 */
 
즉, 이스케이프해야하는 any 문자에 HTML 엔터티를 사용할 수 있으며 실제로 다음을 수행 할 수 있습니다. 

{@link package.class#member label}

/ **
 * 주석의 설명문
 * 다음 메소드 {@link Sample14#setSize(int, int) setSize}를 참조
 * /

```

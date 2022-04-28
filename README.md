# NavigationArchitectureComponent
Android jetpack - [Navigation Component](https://developer.android.com/guide/navigation) Fundamental

## Navigation 
Jetpack에서 제공하는 Navigation Component를 사용하면 하나의 Activity 안에서 여러 Fragment를 항해 하듯이 Fragment간 이동을 쉽게 할 수 있습니다. 

Navigation Component는 아래와 같은 주요 기능들로 구성되어 있습니다. 
- Navigation graph : Navigation 관련 정보가 들어가는 XML입니다. 또 네비게이션과 관련된 작업들을 한 XML에서 관리할 수 있게 합니다. 
- NavHost : Navigation graph를 포함하는 빈 컨테이너 입니다. 
- NavController : Navigation의 라이브러리를 통해 생성된 앱의 navigation을 관리하는 객체입니다. `NavController`는 `NavHost`안에 있는 destination 간의 이동을 조작합니다. 

## Navigation Component Dependencies
Navigation Component를 사용하기 위해서 app level `build.gradle`에 아래와 같이 [dependency](https://developer.android.com/guide/navigation/navigation-getting-started#Set-up)를 추가합니다.

```
dependencies {
  def nav_version = "2.4.2"
  // Kotlin
  implementation "androidx.navigation:navigation-fragment-ktx:$nav_version"
  implementation "androidx.navigation:navigation-ui-ktx:$nav_version"
}
```

화면 이동간 [데이터 전송에 있어서 type-safety](https://developer.android.com/guide/navigation/navigation-getting-started#ensure_type-safety_by_using_safe_args)를 위해서 아래의 dependency와 plugin을 추가합니다.<br>
**project level**
```
dependencies {
    def nav_version = "2.4.2"
    classpath "androidx.navigation:navigation-safe-args-gradle-plugin:$nav_version"
}
```
**app level**
```
plugins {
  id 'androidx.navigation.safeargs.kotlin'
}
```

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

#### ※ Adding dependencies in Android Studio Bumblebee's top level build.gradle ※
Android Studio Bumblebee버전 이상에서는 [top(project) level의 `build.gradle`이 변경](https://developer.android.com/studio/releases/gradle-plugin#settings-gradle)되었습니다. 
해당 버전에서는 아래와 같이 추가하면 됩니다. 
```
plugins {
  id 'androidx.navigation.safeargs.kotlin' version '2.4.2' apply false
}
```

## Navigation Graph 
필요한 dependency와 plugin을 설치했다면 이제 navigation graph를 추가하겠습니다. <br>
resource set 에서 우클릭 후 New > Android Resource File 을 선택합니다. <br>
![image](https://user-images.githubusercontent.com/55622345/165876054-1bbc141d-a2c9-409f-983c-7abee4451679.png)

파일 이름을 설정한 뒤 Resource type을 Navigation으로 놓고 OK를 누릅니다. 
![image](https://user-images.githubusercontent.com/55622345/165876260-02547aa4-0ada-4e82-84e2-a15d3ccf7338.png)

Add Project Dependecy 경고창이 나오면 OK를 누릅니다. (해당 경고창은 필요로 하는 라이브러리들을 자동으로 추가여부를 묻는 창입니다.)

![image](https://user-images.githubusercontent.com/55622345/165876565-330e632c-4bb8-44a3-aae2-e46c29107907.png) <br>
이제 Navigation Graph가 추가가 완료되었습니다. 

## Navigation Host Fragment 
`NavigationHostFragment`를 추가하기 위해서 `activiy_main.xml`로 넘어갑니다. 

우선 Design 화면에서 `FragmentContainerView`를 검색 후 Activiy 화면에 드래그 합니다. (`NavHostFragment`로 추가하여도 `FragmentContainerView`로 자동으로 변환되어 추가됩니다. )<br>
![image](https://user-images.githubusercontent.com/55622345/165877576-b54f1317-24e4-4fb2-98bd-447237c6a41b.png)

Activity에 `FragmentContainerView`가 추가가되면 Navigation Graph 선택 창이 나오는데, 
`FragmentContainerView`를 Design 화면에서 추가함으로 위에서 작성한 Navigation Graph와 쉽게 연결할 수 있습니다. <br>
![image](https://user-images.githubusercontent.com/55622345/165877237-a182dac9-6bb9-40c4-b6cb-57137193ac66.png)


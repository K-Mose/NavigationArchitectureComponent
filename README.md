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

## Navigation Destination
Activity에 Navigation Graph가 추가되었으므로 이제 네비게이션 내에서 화면간 이동에 사용될 Fragment들을 추가하겠습니다. 

생성했던 `nav_graph.xml`로 돌아와 New Destination > Create new destination을 누릅니다. 
![image](https://user-images.githubusercontent.com/55622345/165878114-ebf75943-8866-4d76-baf8-ca45c19dd32f.png)

Fragment 선택창이 나오는데 여기서 사용할 Fragment를 선택 후 **Next**를 눌러 Fragment Name을 지정 후 **Finish**를 누르면 아래와 같이  
Fragment가 생성됩니다. (같은 방법으로 두 번째 Fragment를 추가합니다.) <br>
![image](https://user-images.githubusercontent.com/55622345/165878593-2e987911-0195-40f9-8dd6-12f557ee3167.png)
```xml
<?xml version="1.0" encoding="utf-8"?>
<navigation xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/nav_graph"
    app:startDestination="@id/homeFragment">

    <fragment
        android:id="@+id/homeFragment"
        android:name="com.kmose.navigationarchitecturecomponent.HomeFragment"
        android:label="fragment_home"
        tools:layout="@layout/fragment_home" />
    <fragment
        android:id="@+id/secondFragment"
        android:name="com.kmose.navigationarchitecturecomponent.SecondFragment"
        android:label="fragment_second"
        tools:layout="@layout/fragment_second" />
</navigation>
```
처음 추가된 Fragment가 startDestiantion으로 지정됩니다. id를 변경하여 다른 Fragment로 바꿀 수 있습니다.

Destination을 추가하게 되면 추가된 Fragment의 kt파일과 xml파일이 생성됩니다. <br>
![image](https://user-images.githubusercontent.com/55622345/165878933-b59b592f-3f20-4614-9680-9a672d605610.png)

Frgament간 이동을 준비하기 위해 아래와 같이 각각의 layout을 수정합니다. 

<details>
<summary>Layout-XML</summary>

*fragment_home*
```
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools">
    <androidx.constraintlayout.widget.ConstraintLayout
        android:id="@+id/frameLayout"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        tools:context=".HomeFragment">


        <EditText
            android:id="@+id/etName"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginTop="10dp"
            android:layout_marginStart="16dp"
            android:ems="10"
            android:inputType="textPersonName"
            android:textSize="30sp"
            app:layout_constraintEnd_toEndOf="parent"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintTop_toTopOf="parent"
            app:layout_constraintBottom_toBottomOf="parent"
            app:layout_constraintVertical_bias="0.081"
            />
        <Button
            android:id="@+id/btn_submit"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            app:layout_constraintVertical_bias="0.273"
            android:text="SUBMIT"
            android:textSize="30sp"
            android:backgroundTint="@color/teal_700"
            app:layout_constraintBottom_toBottomOf="parent"
            app:layout_constraintLeft_toLeftOf="parent"
            app:layout_constraintRight_toRightOf="parent"
            app:layout_constraintTop_toTopOf="parent"/>
    </androidx.constraintlayout.widget.ConstraintLayout>
</layout>
```
  

*fragment_second*
```
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">
    <androidx.constraintlayout.widget.ConstraintLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        tools:context=".SecondFragment">
        <TextView
            android:id="@+id/tvName"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="test"
            android:textSize="30sp"
            app:layout_constraintLeft_toLeftOf="parent"
            app:layout_constraintRight_toRightOf="parent"
            app:layout_constraintBottom_toBottomOf="parent"
            app:layout_constraintTop_toTopOf="parent"
            android:layout_marginTop="180dp"
            app:layout_constraintVertical_bias="0"
            />
    </androidx.constraintlayout.widget.ConstraintLayout>
</layout>
```
</details>

***※ DataBinding을 사용함으로 app level `build.gradle`에 아래와 같이 추가합니다.***
```
    buildFeatures {
        dataBinding true
    }
```

DataBinding을 각각의 Fragment에 적용합니다. 
<details>
<summary>Fragment</summary>

*HomeFragment*
```class HomeFragment : Fragment() {
    ……
    private lateinit var binding: FragmentHomeBinding
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        arguments?.let {
            param1 = it.getString(ARG_PARAM1)
            param2 = it.getString(ARG_PARAM2)
        }
    }

    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        // Inflate the layout for this fragment
        binding = DataBindingUtil.inflate(inflater, R.layout.fragment_home, container, false)
        return binding.root
    }
    ……
```
  

*SecondFragment*
```
class SecondFragment : Fragment() {
    ……
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        arguments?.let {
            param1 = it.getString(ARG_PARAM1)
            param2 = it.getString(ARG_PARAM2)
        }
    }

    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        // Inflate the layout for this fragment
        binding = DataBindingUtil.inflate(inflater, R.layout.fragment_second, container, false)
        return binding.root
    }
    ……
```
</details>

## Navigation Action
Fragment간 화면 이동을 위해서 `nav_graph.xml`에 Action을 추가합니다. 
```
    <fragment
        android:id="@+id/homeFragment"
        android:name="com.kmose.navigationarchitecturecomponent.HomeFragment"
        android:label="fragment_home"
        tools:layout="@layout/fragment_home" >
        <action
            android:id="@+id/action_homeFragment_to_secondFragment"
            app:destination="@id/secondFragment" />
    </fragment>
```
Design 화면에서 드래깅으로 쉽게 액션을 추가할 수 있습니다. <br>
![image](https://user-images.githubusercontent.com/55622345/165880996-2afe42cd-ea6b-438c-9d3b-e2ce83227ef9.png)

추가된 Action을 `HomeFragment`의 button의 onClickListener에 추가하겠습니다. 
```kotlin 
    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        // Inflate the layout for this fragment
        binding = DataBindingUtil.inflate(inflater, R.layout.fragment_home, container, false)
        binding.apply {
            btnSubmit.setOnClickListener {
                if(!TextUtils.isEmpty(etName.text.toString())) {
                    val bundle = bundleOf("user_input" to etName.text.toString())
                    it.findNavController().navigate(R.id.action_homeFragment_to_secondFragment, bundle)
                } else {
                    Toast.makeText(activity, "Please input your name", Toast.LENGTH_SHORT).show()
                }
            }
        }
        return binding.root
    }
```
Submit 버튼을 눌렀을 때 Text의 입력값을 확인 후 `Bundle`에 값을 담아 데이터를 전송합니다. `Bundle`은 입력된 key, value 값의 쌍을 `Parcelable`로 변환하여 데이터 전송하도록 도와줍니다. 

전송되는 값을 `SecondFragment`에서 받도록 하겠습니다. 
```kotlin
    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        // Inflate the layout for this fragment
        binding = DataBindingUtil.inflate(inflater, R.layout.fragment_second, container, false)
        val input:String? = requireArguments().getString("user_input")
        binding.tvName.text = input

        return binding.root
    }
```

## Animations For Actions
기본 액션은 애니메이션 효가가 없이 화면이동이 됩니다. 애니메이션을 추가하기 위해서 일단 Action의 기본 속성들을 알아보겠습니다. 

액션 화살표를 클릭하면 아래와 같은 항목들이 나타납니다. 
![image](https://user-images.githubusercontent.com/55622345/165887990-01278a89-9d3b-4355-948e-25bbc3303711.png) <br>
- Enter : destination간 이동 시에 새롭게 화면이 나타나는 애니메이션입니다. 
- Exit : Enter이후 기존 destination이 사라질 때 나타나는 애니메이션 입니다.
- Pop Enter : 이전 destination으로 돌아갈 때 이전 destination의 화면이 나타나는 애니메이션 입니다. 
- Pop : 이전 destination으로 돌아갈 때 현재 화면이 사라지는 애니메이션 입니다. 

[stackoverflow](https://stackoverflow.com/questions/18147840/slide-right-to-left-android-animations) 예제를 이용해서 화면 이동 애니메이션을 추가하겠습니다. ([참고2](https://stackoverflow.com/a/20188089))

resource set에 각각의 slide 애니메이션 xml 파일을 추가합니다. <br>
![image](https://user-images.githubusercontent.com/55622345/165889403-08d0fe5b-e230-47ac-8623-d97b3d2f2648.png)

이제 Action 속성에 애니메이션들을 등록합니다. <br>
![image](https://user-images.githubusercontent.com/55622345/165889743-5032e5ff-8acb-4170-8291-561e604ee42c.png)
```
    <fragment
        android:id="@+id/homeFragment"
        android:name="com.kmose.navigationarchitecturecomponent.HomeFragment"
        android:label="fragment_home"
        tools:layout="@layout/fragment_home" >
        <action
            android:id="@+id/action_homeFragment_to_secondFragment"
            app:destination="@id/secondFragment"
            app:enterAnim="@anim/slide_in_left"
            app:exitAnim="@anim/slide_out_right"
            app:popEnterAnim="@anim/slide_in_right"
            app:popExitAnim="@anim/slide_out_left" />
    </fragment>
```


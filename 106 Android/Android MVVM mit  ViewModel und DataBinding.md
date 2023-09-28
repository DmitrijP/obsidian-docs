
## 1 Gradle

In der Projekt Gradle Datei die DataBindings aktivieren mit Hilfe von ``dataBinding true``

```groovy
plugins {  
    id 'com.android.application'  
}  
  
android {  
    namespace 'de.dmitrijpatuk.simpledms'  
    compileSdk 33  
  
    defaultConfig {  
	    ...
    }  
    buildFeatures {  
        dataBinding true  #<-------------- dataBinding true eintragen um die DataBindings zu aktivieren!
    }  
    buildTypes {  
        ... 
    }    
    compileOptions {  
        sourceCompatibility JavaVersion.VERSION_11  
        targetCompatibility JavaVersion.VERSION_11  
    }  
}  
  
dependencies {  
  ...
}
```

## 2. ViewModel

Das ViewModel muss von ViewModel erben. Es enthält die Logik der Applikation.

```java
public class MainViewModel extends ViewModel {  
	
    private MutableLiveData<String> text = new MutableLiveData<>();  
    public LiveData<String> getText() {  
        return text;  
    }  
  
    public void initialize(){  
        text.postValue("Was lauft?");  
    }  
  
    public void onClick(){  
        Log.i(MainViewModel.class.getSimpleName(), "Click");  
        text.postValue("Hallo");  
    }  
}
```

## 3. View

Die View muss in ``<layout>`` Tags liegen. Dazu benötig man ein de Variablendefinition des ViewModels.
ClickBinding: ``android:onClick="@{() -> vm.onClick()}" ``
TextBinding:  ``android:text="@{vm.text}"``
```xml
<?xml version="1.0" encoding="utf-8"?>  
<layout>  
    <data>        
	    <variable name="vm" type="de.dmitrijpatuk.simpledms.MainViewModel" />  
    </data>    
	<androidx.constraintlayout.widget.ConstraintLayout     
		xmlns:android="http://schemas.android.com/apk/res/android"  
        xmlns:app="http://schemas.android.com/apk/res-auto"  
        xmlns:tools="http://schemas.android.com/tools"  
        android:layout_width="match_parent"  
        android:layout_height="match_parent"  
        tools:context=".MainActivity">  
  
        <Button            
	        android:id="@+id/hello_button"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:text="@string/hello_world_button_text"  
            android:onClick="@{() -> vm.onClick()}"  
            app:layout_constraintEnd_toEndOf="parent"  
            app:layout_constraintStart_toStartOf="parent"  
            app:layout_constraintTop_toTopOf="parent" />  
  
        <TextView            
	        android:id="@+id/hello_textview"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:text="@{vm.text}"  
            app:layout_constraintEnd_toEndOf="parent"  
            app:layout_constraintStart_toStartOf="parent"  
            app:layout_constraintTop_toBottomOf="@id/hello_button" />  
            
    </androidx.constraintlayout.widget.ConstraintLayout>  
</layout>
```

## 4. Activity

```java
public class MainActivity extends AppCompatActivity {  
    @Override  
    protected void onCreate(Bundle savedInstanceState) {  
        super.onCreate(savedInstanceState);  
        var binding =  ActivityMainBinding.inflate(getLayoutInflater());  
        var vm = new ViewModelProvider(this).get(MainViewModel.class);  
        setContentView(binding.getRoot());  
        binding.setVm(vm);  
        binding.setLifecycleOwner(this);  // <======EXTREM WICHTIG!!!!
    }  
}
```
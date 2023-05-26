# Explained-MVC-MVP-and-MVVM-in-android-app-using-kotlin

## MVC (Model-View-Controller):

- Model represents the data and business logic of the application.
- View handles the presentation layer and user interface.
- Controller acts as the intermediary between the model and the view, handling user input, updating the model, and updating the view.

## MVP (Model-View-Presenter):

- Model represents the data and business logic of the application.
- View defines the contract for the user interface and receives updates from the presenter.
- Presenter acts as the intermediary between the model and the view, handling user actions, updating the model, and updating the view. The presenter contains the logic and state management.

## MVVM (Model-View-ViewModel):

- Model represents the data and business logic of the application.
- View displays the UI and interacts with the user.
- ViewModel exposes data and methods to the view, maintaining the state and handling the logic. The ViewModel is responsible for providing data to the view and reacting to user actions. It uses data binding or observable patterns to keep the view in sync with the ViewModel.

# Key Differences:

- **Responsibility:** In MVC, the controller is responsible for handling user input and updating the model and view. In MVP, the presenter handles user actions, updates the model, and updates the view. In MVVM, the ViewModel exposes data and methods to the view, which reacts to user actions.

- **Dependency Flow:** In MVC, the view has a reference to the model, and the controller communicates with both the model and view. In MVP, the view has a reference to the presenter, and the presenter has a reference to the model. In MVVM, the view has a reference to the ViewModel, and the ViewModel has a reference to the model.

- **State Management:** In MVC, the state is managed by the model, and the view and controller fetch the data from the model. In MVP, the state is managed by the presenter, and the view reflects the state provided by the presenter. In MVVM, the state is managed by the ViewModel, and the view binds to the ViewModel's properties to reflect the state.

- **Testability:** MVP and MVVM are often considered more testable than MVC because the business logic is isolated in the presenter/ViewModel, making it easier to write unit tests without direct dependencies on the view or framework components.

It's important to note that there isn't a "one-size-fits-all" solution, and the choice of design pattern depends on the specific requirements and complexities of the application.

Now let us create a counter android app using above three design patterns and find the difference.

## Counter App Using MVC

```kt
// MainActivity.kt
package com.example.mvcmvpmvvm

import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle

class MainActivity : AppCompatActivity() {

    private lateinit var counterController: CounterController
    private lateinit var counterModel: CounterModel
    private lateinit var counterView: CounterView

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        counterView = findViewById(R.id.counterView)
        counterModel = CounterModel()
        counterController = CounterController(counterModel, counterView)

    }
}
```

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--activity_main.xml-->
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <com.example.mvcmvpmvvm.CounterView
        android:id="@+id/counterView"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        />

</androidx.constraintlayout.widget.ConstraintLayout>
```

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--view_counter.xml-->
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:gravity="center">

    <TextView
        android:id="@+id/countTextView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:textSize="24sp"
        android:textColor="@android:color/black"
        android:text="0" />

    <Button
        android:id="@+id/incrementButton"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Increment" />

    <Button
        android:id="@+id/decrementButton"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Decrement" />

</LinearLayout>
```

let's create the CounterModel class, which represents the data and business logic of the counter:

```kt
class CounterModel {
    private var count: Int = 0

    fun getCount(): Int {
        return count
    }

    fun incrementCount() {
        count++
    }

    fun decrementCount() {
        count--
    }
}
```

Next, we'll create the CounterView class, which handles the user interface:

```kt
import android.content.Context
import android.util.AttributeSet
import android.widget.Button
import android.widget.LinearLayout
import android.widget.TextView

class CounterView @JvmOverloads constructor(
    context: Context,
    attrs: AttributeSet? = null,
    defStyleAttr: Int = 0
) : LinearLayout(context, attrs, defStyleAttr) {
    private lateinit var countTextView: TextView
    private lateinit var incrementButton: Button
    private lateinit var decrementButton: Button

    private var controller: CounterController? = null

    init {
        inflate(context, R.layout.view_counter, this)

        countTextView = findViewById(R.id.countTextView)
        incrementButton = findViewById(R.id.incrementButton)
        decrementButton = findViewById(R.id.decrementButton)

        incrementButton.setOnClickListener {
            controller?.incrementCount()
        }

        decrementButton.setOnClickListener {
            controller?.decrementCount()
        }
    }

    fun setController(controller: CounterController) {
        this.controller = controller
    }

    fun updateCount(count: Int) {
        countTextView.text = count.toString()
    }
}
```

Finally, we'll create the CounterController class, which acts as an intermediary between the model and the view:

```kt
class CounterController(private val model: CounterModel, private val view: CounterView) {
    init {
        view.setController(this)
        updateView()
    }

    fun incrementCount() {
        model.incrementCount()
        updateView()
    }

    fun decrementCount() {
        model.decrementCount()
        updateView()
    }

    private fun updateView() {
        val count = model.getCount()
        view.updateCount(count)
    }
}
```

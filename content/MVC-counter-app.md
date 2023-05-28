Here's an example of a simple Android counter app that demonstrates the Model-View-Controller (MVC) design pattern.

First, let's start with the Model. In MVC, the Model represents the data and the business logic of the application. In this case, the Model will contain the counter value and the methods to manipulate it.

```java
public class CounterModel {
    private int counter;

    public CounterModel() {
        counter = 0;
    }

    public int getCounter() {
        return counter;
    }

    public void incrementCounter() {
        counter++;
    }

    public void decrementCounter() {
        counter--;
    }
}
```

Next, let's move on to the View. The View is responsible for displaying the user interface and capturing user input. In this example, we'll use a simple layout with two buttons to increment and decrement the counter, and a text view to display the current counter value.

```xml
<!-- activity_main.xml -->
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <Button
        android:id="@+id/btnIncrement"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Increment" />

    <TextView
        android:id="@+id/tvCounter"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:textSize="24sp"
        android:text="0" />

    <Button
        android:id="@+id/btnDecrement"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Decrement" />

</LinearLayout>
```

Now, let's create the Controller. The Controller acts as the intermediary between the Model and the View. It listens to user input events from the View and updates the Model accordingly.

```js
public class CounterController implements View.OnClickListener {
    private CounterModel model;
    private TextView counterTextView;

    public CounterController(CounterModel model, TextView counterTextView) {
        this.model = model;
        this.counterTextView = counterTextView;
    }

    @Override
    public void onClick(View view) {
        switch (view.getId()) {
            case R.id.btnIncrement:
                model.incrementCounter();
                break;
            case R.id.btnDecrement:
                model.decrementCounter();
                break;
        }
        updateCounterView();
    }

    private void updateCounterView() {
        counterTextView.setText(String.valueOf(model.getCounter()));
    }
}
```

Finally, let's integrate the Model, View, and Controller in the main activity.

```java
public class MainActivity extends AppCompatActivity {
    private CounterModel counterModel;
    private TextView counterTextView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        counterModel = new CounterModel();
        counterTextView = findViewById(R.id.tvCounter);

        Button btnIncrement = findViewById(R.id.btnIncrement);
        Button btnDecrement = findViewById(R.id.btnDecrement);

        CounterController counterController = new CounterController(counterModel, counterTextView);
        btnIncrement.setOnClickListener(counterController);
        btnDecrement.setOnClickListener(counterController);
    }
}
```

That's it! This example demonstrates a simple Android counter app using the MVC design pattern. The Model represents the counter value and provides methods to manipulate it. The View displays the user interface, and the Controller handles user input events and updates the Model and View accordingly.


# International Hellenic University  -  1st lesson overview

### Introduction
In order to run your application directly on your mobile phone, you must enable USB Debugging.
How to do it: https://www.youtube.com/watch?v=NtrrtkSentA


### We will create from scratch an application for presenting weather forecasts in a list

![alt tag](https://github.com/IHUAndroid/IHU_Lesson01-material/blob/master/ihu-sunshine-1-dummy-screen.png)


### Instructions
- We start from mockup design first. Who do we want the graphics of the forecast?

- ```MainActivity``` is the basic class from where everything starts. Inside there we will create the inner class  ```PlaceholderFragment```. 
Fragment is a modular container that can be reused in many other activities.

We shoud also change ```MainActivity.onCreate()``` to be as follows:

````
@Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        if (savedInstanceState == null) {
            getSupportFragmentManager().beginTransaction()
                    .add(R.id.container, new PlaceholderFragment())
                    .commit();
        }
    }
````

- The forecast list will be implemented as a ```ListView```. Every ```ListView``` fills with many ```ListItems```. For this reason we should define how the ```ListItem``` will be depicted


- We add in the folder res/layout the file ```list_item_forecast.xml``` (res->layout-> right click-> New -> Layout Resource file ) 
This file currently describes a TextView  with ID ```@+id/list_item_forecast_textview```. 

The full file contents :

```
<?xml version="1.0" encoding="utf-8"?>
<TextView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:minHeight="?android:attr/listPreferredItemHeight"
    android:gravity="center_vertical"
    android:id="@+id/list_item_forecast_textview">
</TextView>
```
Pay attention to:
```
android:layout_height="wrap_content"
android:minHeight="?android:attr/listPreferredItemHeight"
android:gravity="center_vertical"
```

### Responsive design
- Introduction to Responsive design. Why it is so important? (ans: various sizes of devices and screens).
Different way of thinking and creating responsive layouts. Examples (http://rollpark.us/, https://www.wired.com/ http://www.theatlantic.com/world/ ).

- Layout Managers: Frame Layout, Linear Layout, Relative Layout

- FrameLayout: Ideal for occasions where the screen will host only one component, e.g. a list and nothing more.

- LinearLayout: Ideal for stacking components, one next to other or one above an other. **Also it can help us divide the screen to smaller areas**

- RelativeLayout: Powerfull but difficult. Every component's place is defined in relation to some other component. 


**Lists in mobile devices**

- ScrollView. What can go wrong? How many items can fit on the screen? How many items fit in memory? For example, if a screen holds 10 items and there are 200, how many should i load?

- ListView to the rescue. It dynamically loads list items and create each just before it appears on screen. 

- And what about already seen items that are now "above" the screen? They keep eating memory! Recycler view to the rescue

- MVC = Model View Controller 
- Adapter Pattern. 

- AdapterView: Adapter loads data to View without caring about the specific type of View. 

**We will gona use ListView**

- Implementing ListView: Modify ```fragment_main.xml``` so that: 
  1. Displays a  ```ListView``` instead of a ```TextView```
  2. Uses a ```FrameLayout``` instead of a ```RelativeLayout```

- ListView xml should contain:
```
<ListView
android:id="@+id/listview_forecast"
android:layout_width="match_parent"
android:layout_height="match_parent" />

```
-Now is the time to fill ListView with data. Open ```MainActivity.java```.

- In the inner class ```PlaceholderFragment```, in the method ```onCreateView(...)``` create an array with dummy data. 

You can use this, if you want:

```
String[] data = {
                    "Mon 6/23 - Sunny - 31/17",
                    "Tue 6/24 - Foggy - 21/8",
                    "Wed 6/25 - Cloudy - 22/17",
                    "Thurs 6/26 - Rainy - 18/11",
                    "Fri 6/27 - Foggy - 21/10",
                    "Sat 6/28 - Error! - 23/18",
                    "Sun 6/29 - Sunny - 20/7"         };
```

- ListView data flow. (Relative video: https://www.youtube.com/watch?v=uOOupBYZeO0)

![ListView](https://github.com/UomMobileDevelopment/Lesson02-material/blob/master/listViewDataHandlingModel.PNG)

- Implement method ```onCreateView(...)``` to create the ```Adapter```. In our case, we need an ```ArrayAdapter```. The contructor takes 4 parameters.
  1. Context. Includes all info regarding the running environment and gives access to system files and core services (```getActivity()```)
  2. ID of the list item layout. We will gain access to this through the globally accessble file ```R.java```. This file contains human-readable names (instead of memory addresses) for all of our resources. It exists **always** in any android project (```R.layout.list_item_forecast```)
  3. ID of the text view (```R.id.list_item_forecast_textview```)
  4. Data list (the list with the dummy data for now)
  
Let's pay attention to the names inside ```R.java```. There is a name ```R.layout``` and a name  ```R.id```.
This indicates that the first variable (.layout) refers to an xml layout resource, while the second refers to a specific xml component.

### Cconnecting the ```Adapter``` with the ```ListView```

- To begin with, we should gain access to a reference to the ```ListView``` by using the given ID  (```@+id/listview_forecast```). (through ```R.java```). 
*Some info about finding views by using their IDs ```findViewById()```*

![Finding Views](https://github.com/UomMobileDevelopment/Lesson02-material/blob/master/findingViews.PNG)

The code that we should write is:
```
            ListView forecastListView = (ListView)rootView.findViewById(R.id.listview_forecast);
            forecastListView.setAdapter(forecastListAdapter);
```

and the whole picture:

```
public static class PlaceholderFragment extends Fragment {

        public PlaceholderFragment() {
        }

        @Override
        public View onCreateView(LayoutInflater inflater, ViewGroup container,
                                 Bundle savedInstanceState) {
            View rootView = inflater.inflate(R.layout.fragment_main, container, false);
            // Create some dummy data for the ListView.  Here's a sample weekly forecast
            String[] data = {
                    "Mon 6/23 - Sunny - 31/17",
                    ///........
                    "Sun 6/29 - Sunny - 20/7"
            };
            List<String> weekForecast = new ArrayList<String>(Arrays.asList(data));

            ArrayAdapter<String> forecastListAdapter =
                    new ArrayAdapter<String>(
                            getActivity(),
                            R.layout.list_item_forecast,
                            R.id.list_item_forecast_textview, weekForecast);

            ListView forecastListView = (ListView)rootView.findViewById(R.id.listview_forecast);
            forecastListView.setAdapter(forecastListAdapter);


            return rootView;
        }
    }
```

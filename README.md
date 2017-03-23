# International Hellenic University  -  1st lesson overview

### Introduction
In order to run your application directly on your mobile phone, you must enable USB Debugging.
How to do it: https://www.youtube.com/watch?v=NtrrtkSentA


### We will create from scratch an application for presenting weather forecasts in a list

![alt tag](https://github.com/UomMobileDevelopment/Lesson02-material/blob/master/Shunshine-dummy-screen-smaller.png)


### Instructions
- We start from mockup design first. Who do we want the graphics of the forecast?

- ```MainActivity``` is the basic class from where everything starts. Inside there is the inner class  ```PlaceholderFragment```. 
Fragment is a modular container that can be reused in many other activities.

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

- Επεξήγηση ροής δεδομένων στο ListView. (Σχετικό βίντεο: https://www.youtube.com/watch?v=uOOupBYZeO0)

![ListView](https://github.com/UomMobileDevelopment/Lesson02-material/blob/master/listViewDataHandlingModel.PNG)

- Γράφουμε κώδικα στη μέθοδο ```onCreateView(...)``` για να φτιάξουμε τον ```Adapter```. Στην περίπτωσή μας χρειαζόμαστε έναν ```ArrayAdapter```. Ο κατασκευαστής του παίρνει 4 παραμέτρους.
  1. Το context. Περιλαμβάνει όλες τις πληροφορίες για το περιβάλλον εκτέλεσης και δίνει πρόσβαση σε υπηρεσίες και αρχεία του συστήματος  (```getActivity()```)
  2. ID του list item layout. Θα το πάρουμε μέσω του καθολικού και ειδικού αρχείου R.java. Αυτό το αρχείο παρέχει ανθρώπινες ονομασίες (αντί για διευθύνσεις μνήμης) για όλα τα resources μας. Υπάρχει πάντα σε κάθε android project (```R.layout.list_item_forecast```)
  3. ID του text view (```R.id.list_item_forecast_textview```)
  4. Τη λίστα με τα δεδομένα (όπως ονομάσαμε το ArrayList)
  
Ας δώσουμε λίγη προσοχή στις ονομασίες απο το αρχείο R.java. Η μία ξεκινά με ```R.layout``` ενώ η άλλη με ```R.id```
αυτό υποδυκνύει πως η πρώτη μεταβλητή αναφέρεται σε αρχείο διάταξης xml ενώ η δεύτερη αναφέρεται σε συγκεκριμένο συστατικό xml.

- Κώδικας για να συνδέσουμε με τον ```Adapter``` με το ```ListView```. Αρχικά να 'πάρουμε' στα χέρια μας μία αναφορά προς το ListView χρησιμοποιώντας το ID που της έχουμε δώσει (```@+id/listview_forecast```). Υπενθύμιση η πρόσβαση στα ID μέσα απο τον κώδικα γίνεται με τη χρήση του ```R.java```. 
Λίγα λόγια για το πώς βρίσκουμε τα Views Με τη μέθodo ```findViewById()```

![Finding Views](https://github.com/UomMobileDevelopment/Lesson02-material/blob/master/findingViews.PNG)

Έτσι, ο κώδικας που πρέπει να γράψουμε είναι:
```
            ListView forecastListView = (ListView)rootView.findViewById(R.id.listview_forecast);
            forecastListView.setAdapter(forecastListAdapter);
```

και έτσι έχουμε συνολικά:

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

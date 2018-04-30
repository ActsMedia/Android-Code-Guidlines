# ActsMedia Android design guildines

# Organization:

## Code:
### **Classes should be organized by usage, then type of utility or section of the application when necessary.**

This allows quick navigation when moving between projects and simplifies naming when classes are re-used in different parts of an application

To avoid long lists of files at the same level, try to limit files in a folder to less than 10 (not including folders).

**Example:**

    basePackage
        ->Activity
            MainActivity
            DetailsActivity
        ->Fragment
            ->Details
                DetailsHeaderFragment
                DetailsContentFragment
            MainListFragment
        ->Views
            RoundedImageView
        ->Adapters
            MainListAdapter
        ->Model
            ->ViewModel
            ->Database
            ProductSharedPreferences
        ->Controller
            FileManager
        ->Utilities
            StringUtility
            DateUtility


## Resources

### **Resource files should be named with a scheme of: `layoutType_appSection_specificName.xml`**

This allows for easier navigation when finding layouts and results in related resources being automatically grouped together by Android Studio


**Examples:**

```
activity_main.xml
activity_user_sign_up.xml

fragment_main.xml
fragment_user_sign_up.xml

row_main.xml
row_user_sign_up.xml

cell_main.xml
cell_user_sign_up_xml
```

# UI Design

### **The bulk of the UI elements and interaction from the user should take place in Fragments, broken up by purpose**

This allows easier re-use of both layouts and interaction logic between screens, and also avoids tying the UI to a specific Activity.

# Class Design

### **Any information needed by Fragments and Activies should be defined in Constructors within the Class, and any navigation should use these Constructors**

This allows the Class to define it's own dependencies and simple Code Completion to discover those dependencies. 

**Examples**

```
class ExampleFragment: Fragment() {
    companion object {
        private const val ID_KEY = "ID_KEY"
        fun newInstance(modelID: Long): ExampleFragment {
            val fragment = ExampleFragment()
            fragment.arguments = makeBundle(modelID)
            return fragment
        }

        private fun makeBundle(modelID: Long): Bundle {
            val bundle = Bundle()
            bundle.putParcelable(ID_KEY, modelID)
            return bundle
        }
    
        private fun getModel(bundle: Bundle): Long {
            val modelValue: Long = bundle.getParcelable(ID_KEY)
            return modelValue
        }
    }
}

class ExampleActivity: Activity() {
    companion object {
        private const val ID_KEY = "ID_KEY"
        fun makeIntent(context: Context, modelID: Long): Intent {
            val intent = Intent(context, ExampleActivity::class.java)
                        .putExtra(ID_KEY, modelID)
            return intent
        }

        private fun getModelFromIntent(intent: Intent): Long {
            return intent.getSerializableExtra(MODEL_PARAM_KEY) as Long
        }
    }
}
```

### **Basic Transitions between Activities should use the Activity Extensions in :**

This gives the app a consistent feel when moving through the app and a defined set of animations without having to set them for every transitions.


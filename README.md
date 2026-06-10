Splash
package com.example.campsitecommander

import android.content.Intent
import android.os.Bundle
import android.os.Handler
import android.os.Looper
import androidx.appcompat.app.AppCompatActivity

class SplashActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_splash)

        // 3 seconds = 3000ms then go to Main
        Handler(Looper.getMainLooper()).postDelayed({
            startActivity(Intent(this, MainActivity::class.java))
            finish()
        }, 3000)
    }
}


Main
package com.example.campsitecommander

import android.content.Intent
import android.os.Bundle
import android.widget.Button
import android.widget.TextView
import androidx.appcompat.app.AppCompatActivity

class MainActivity : AppCompatActivity() {

    companion object {
        // Parallel arrays - shared across all activities
        var itemNames = arrayOf("Tent", "Marshmallows", "Flashlight")
        var categories = arrayOf("Shelter", "Food", "Safety")
        var quantities = intArrayOf(1, 3, 2)
        var comments = arrayOf("4-person waterproof", "For S'mores (Mega size)", "Check batteries (AA)")
    }

    private lateinit var textViewTotal: TextView

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        textViewTotal = findViewById(R.id.textViewTotal)
        val btnAddGear = findViewById<Button>(R.id.btnAddGear)
        val btnViewList = findViewById<Button>(R.id.btnViewList)

        updateTotal()

        btnAddGear.setOnClickListener {
            startActivity(Intent(this, AddGearActivity::class.java))
        }

        btnViewList.setOnClickListener {
            startActivity(Intent(this, DetailedViewActivity::class.java))
        }
    }

    override fun onResume() {
        super.onResume()
        updateTotal() // refresh total when coming back
    }

    // Loop to calculate total items packed
    private fun updateTotal() {
        var total = 0
        for (qty in quantities) {
            total += qty
        }
        textViewTotal.text = "Total Items Packed: $total"
    }
}

Add Gear
package com.example.campsitecommander

import android.os.Bundle
import android.widget.Button
import android.widget.EditText
import android.widget.Toast
import androidx.appcompat.app.AppCompatActivity
import com.example.campsitecommander.MainActivity.Companion.categories
import com.example.campsitecommander.MainActivity.Companion.comments
import com.example.campsitecommander.MainActivity.Companion.itemNames
import com.example.campsitecommander.MainActivity.Companion.quantities

class AddGearActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_add_gear)

        val editItemName = findViewById<EditText>(R.id.editItemName)
        val editCategory = findViewById<EditText>(R.id.editCategory)
        val editQuantity = findViewById<EditText>(R.id.editQuantity)
        val editComments = findViewById<EditText>(R.id.editComments)
        val btnSaveGear = findViewById<Button>(R.id.btnSaveGear)
        val btnBackToMain = findViewById<Button>(R.id.btnBackToMain)

        btnSaveGear.setOnClickListener {
            val name = editItemName.text.toString().trim()
            val category = editCategory.text.toString().trim()
            val qtyStr = editQuantity.text.toString().trim()
            val comment = editComments.text.toString().trim()

            // Error handling for empty inputs
            if (name.isEmpty() || category.isEmpty() || qtyStr.isEmpty() || comment.isEmpty()) {
                Toast.makeText(this, "Please fill all fields!", Toast.LENGTH_SHORT).show()
                return@setOnClickListener
            }

            val qty = qtyStr.toIntOrNull()
            if (qty == null || qty <= 0) {
                Toast.makeText(this, "Quantity must be a number > 0", Toast.LENGTH_SHORT).show()
                return@setOnClickListener
            }

            // Add to parallel arrays
            itemNames = itemNames + name
            categories = categories + category
            quantities = quantities + qty
            comments = comments + comment

            Toast.makeText(this, "$name added to packing list!", Toast.LENGTH_SHORT).show()
            finish() // go back to main
        }

        btnBackToMain.setOnClickListener {
            finish()
        }
    }
}

Detailed
package com.example.campsitecommander

import android.os.Bundle
import android.widget.Button
import android.widget.TextView
import androidx.appcompat.app.AppCompatActivity
import com.example.campsitecommander.MainActivity.Companion.categories
import com.example.campsitecommander.MainActivity.Companion.comments
import com.example.campsitecommander.MainActivity.Companion.itemNames
import com.example.campsitecommander.MainActivity.Companion.quantities

class DetailedViewActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_detailed_view)

        val textViewList = findViewById<TextView>(R.id.textViewList)
        val btnBack = findViewById<Button>(R.id.btnBack)

        // Loop through parallel arrays to display checklist
        val output = StringBuilder()
        if (itemNames.isEmpty()) {
            output.append("No items packed yet.")
        } else {
            for (i in itemNames.indices) {
                output.append("${i + 1}. ${itemNames[i]}\n")
                output.append("Category: ${categories[i]}\n")
                output.append("Quantity: ${quantities[i]}\n")
                output.append("Note: ${comments[i]}\n")
                output.append("--------------------\n")
            }
        }
        textViewList.text = output.toString()

        btnBack.setOnClickListener {
            finish() // "Back to Base" navigation
        }
    }
}

Splash launcher
<activity
    android:name=".SplashActivity"
    android:exported="true">
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>


    xml add gear :
    
Luyanda Ross
13:21 (0 minutes ago)
to me

package com.example.campsitecommander

import android.os.Bundle
import android.widget.Button
import android.widget.EditText
import android.widget.Toast
import androidx.appcompat.app.AppCompatActivity
import com.example.campsitecommander.MainActivity.Companion.categories
import com.example.campsitecommander.MainActivity.Companion.comments
import com.example.campsitecommander.MainActivity.Companion.itemNames
import com.example.campsitecommander.MainActivity.Companion.quantities

class AddGearActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_add_gear)

        val editItemName = findViewById<EditText>(R.id.editItemName)
        val editCategory = findViewById<EditText>(R.id.editCategory)
        val editQuantity = findViewById<EditText>(R.id.editQuantity)
        val editComments = findViewById<EditText>(R.id.editComments)
        val btnSaveGear = findViewById<Button>(R.id.btnSaveGear)
        val btnBackToMain = findViewById<Button>(R.id.btnBackToMain)

        btnSaveGear.setOnClickListener {
            val name = editItemName.text.toString().trim()
            val category = editCategory.text.toString().trim()
            val qtyStr = editQuantity.text.toString().trim()
            val comment = editComments.text.toString().trim()

            // Error handling for empty inputs
            if (name.isEmpty() || category.isEmpty() || qtyStr.isEmpty() || comment.isEmpty()) {
                Toast.makeText(this, "Please fill all fields!", Toast.LENGTH_SHORT).show()
                return@setOnClickListener
            }

            val qty = qtyStr.toIntOrNull()
            if (qty == null || qty <= 0) {
                Toast.makeText(this, "Quantity must be a number > 0", Toast.LENGTH_SHORT).show()
                return@setOnClickListener
            }

            // Add to parallel arrays
            itemNames = itemNames + name
            categories = categories + category
            quantities = quantities + qty
            comments = comments + comment

            Toast.makeText(this, "$name added to packing list!", Toast.LENGTH_SHORT).show()
            finish() // go back to main
        }

        btnBackToMain.setOnClickListener {
            finish()
        }
    }
}

</activity>
<activity android:name=".MainActivity" />
<activity android:name=".AddGearActivity" />
<activity android:name=".DetailedViewActivity" />

<?xml version="1.0" encoding="utf-8"?>
<ScrollView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#0F2027">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical"
        android:padding="24dp">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Add New Gear"
            android:textColor="#FFFFFF"
            android:textSize="24sp"
            android:textStyle="bold" />

        <EditText
            android:id="@+id/editItemName"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="20dp"
            android:hint="Item Name e.g. Tent"
            android:textColor="#FFFFFF"
            android:textColorHint="#888" />

        <EditText
            android:id="@+id/editCategory"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="12dp"
            android:hint="Category e.g. Shelter, Food, Safety"
            android:textColor="#FFFFFF"
            android:textColorHint="#888" />

        <EditText
            android:id="@+id/editQuantity"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="12dp"
            android:hint="Quantity"
            android:inputType="number"
            android:textColor="#FFFFFF"
            android:textColorHint="#888" />

        <EditText
            android:id="@+id/editComments"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="12dp"
            android:hint="Comments e.g. 4-person waterproof"
            android:textColor="#FFFFFF"
            android:textColorHint="#888" />

        <Button
            android:id="@+id/btnSaveGear"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="24dp"
            android:backgroundTint="#2C5364"
            android:text="Save Gear"
            android:textColor="#FFFFFF" />

        <Button
            android:id="@+id/btnBackToMain"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="8dp"
            android:backgroundTint="#203A43"
            android:text="Back to Base"
            android:textColor="#FFFFFF" />

    </LinearLayout>
</ScrollView>





NEW
<color name="dark_bg">#0F2027</color><color name="card_bg">#1B262C</color><color name="accent">#FFD700</color>

Activity main<?xml version="1.0" encoding="utf-8"?><LinearLayout    xmlns:android="http://schemas.android.com/apk/res/android"    android:layout_width="match_parent"    android:layout_height="match_parent"    android:orientation="vertical"    android:padding="20dp"    android:gravity="center">
    <TextView        android:id="@+id/txtTitle"        android:layout_width="wrap_content"        android:layout_height="wrap_content"        android:text="Campsite Commander"        android:textSize="28sp"        android:textStyle="bold"/>
    <EditText        android:id="@+id/edtItem"        android:layout_width="match_parent"        android:layout_height="wrap_content"        android:hint="Item Name"/>
    <EditText        android:id="@+id/edtCategory"        android:layout_width="match_parent"        android:layout_height="wrap_content"        android:hint="Category"/>
    <EditText        android:id="@+id/edtQuantity"        android:layout_width="match_parent"        android:layout_height="wrap_content"        android:hint="Quantity"        android:inputType="number"/>
    <Button        android:id="@+id/btnAdd"        android:layout_width="match_parent"        android:layout_height="wrap_content"        android:text="Add Gear"/>
    <TextView        android:id="@+id/txtTotal"        android:layout_width="wrap_content"        android:layout_height="wrap_content"        android:text="Total Items: 0"        android:textSize="20sp"        android:layout_marginTop="20dp"/>
    <Button        android:id="@+id/btnDetails"        android:layout_width="match_parent"        android:layout_height="wrap_content"        android:text="View Details"/>
</LinearLayout>
Detail<?xml version="1.0" encoding="utf-8"?><LinearLayout    xmlns:android="http://schemas.android.com/apk/res/android"    android:layout_width="match_parent"    android:layout_height="match_parent"    android:orientation="vertical"    android:padding="16dp">
    <TextView        android:id="@+id/txtDetails"        android:layout_width="match_parent"        android:layout_height="0dp"        android:layout_weight="1"        android:textSize="18sp"/>
    <Button        android:id="@+id/btnBack"        android:layout_width="match_parent"        android:layout_height="wrap_content"        android:text="Back to Base"/>
</LinearLayout>
Splash<?xml version="1.0" encoding="utf-8"?><LinearLayout    xmlns:android="http://schemas.android.com/apk/res/android"    android:layout_width="match_parent"    android:layout_height="match_parent"    android:gravity="center"    android:orientation="vertical">
    <ImageView        android:layout_width="150dp"        android:layout_height="150dp"        android:src="@mipmap/ic_launcher"/>
    <TextView        android:layout_width="wrap_content"        android:layout_height="wrap_content"        android:text="Campsite Commander"        android:textSize="28sp"        android:textStyle="bold"/>
</LinearLayout>
Splashkt
package com.example.campsitecommander
import android.content.Intentimport android.os.Bundleimport android.os.Handlerimport android.os.Looperimport androidx.appcompat.app.AppCompatActivity
class SplashActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {        super.onCreate(savedInstanceState)        setContentView(R.layout.activity_splash)
        Handler(Looper.getMainLooper()).postDelayed({
            startActivity(                Intent(this, MainActivity::class.java)            )
            finish()
        }, 3000)    }}
Mainpackage com.example.campsitecommander
import android.content.Intentimport android.os.Bundleimport android.widget.*import androidx.appcompat.app.AppCompatActivity
class MainActivity : AppCompatActivity() {
    private val itemNames = ArrayList<String>()    private val categories = ArrayList<String>()    private val quantities = ArrayList<Int>()    private val comments = ArrayList<String>()
    override fun onCreate(savedInstanceState: Bundle?) {        super.onCreate(savedInstanceState)        setContentView(R.layout.activity_main)
        val edtItem = findViewById<EditText>(R.id.edtItem)        val edtCategory = findViewById<EditText>(R.id.edtCategory)        val edtQuantity = findViewById<EditText>(R.id.edtQuantity)
        val btnAdd = findViewById<Button>(R.id.btnAdd)        val btnDetails = findViewById<Button>(R.id.btnDetails)        val txtTotal = findViewById<TextView>(R.id.txtTotal)
        // Sample Data        itemNames.add("Tent")        categories.add("Shelter")        quantities.add(1)        comments.add("4-person waterproof")
        itemNames.add("Marshmallows")        categories.add("Food")        quantities.add(3)        comments.add("For S'mores")
        itemNames.add("Flashlight")        categories.add("Safety")        quantities.add(2)        comments.add("Check batteries")
        updateTotal(txtTotal)
        btnAdd.setOnClickListener {
            if (                edtItem.text.isEmpty() ||                edtCategory.text.isEmpty() ||                edtQuantity.text.isEmpty()            ) {
                Toast.makeText(                    this,                    "Please complete all fields",                    Toast.LENGTH_SHORT                ).show()
            } else {
                itemNames.add(edtItem.text.toString())                categories.add(edtCategory.text.toString())                quantities.add(edtQuantity.text.toString().toInt())                comments.add("User Added")
                updateTotal(txtTotal)
                Toast.makeText(                    this,                    "Item Added",                    Toast.LENGTH_SHORT                ).show()
                edtItem.text.clear()                edtCategory.text.clear()                edtQuantity.text.clear()            }        }
        btnDetails.setOnClickListener {
            val intent = Intent(this, DetailActivity::class.java)
            intent.putStringArrayListExtra("items", itemNames)            intent.putStringArrayListExtra("categories", categories)            intent.putIntegerArrayListExtra("quantities", quantities)            intent.putStringArrayListExtra("comments", comments)
            startActivity(intent)        }    }
    private fun updateTotal(txtTotal: TextView) {
        var total = 0
        for (qty in quantities) {            total += qty        }
        txtTotal.text = "Total Items: $total"    }}
Detailpackage com.example.campsitecommander
import android.os.Bundleimport android.widget.Buttonimport android.widget.TextViewimport androidx.appcompat.app.AppCompatActivity
class DetailActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {        super.onCreate(savedInstanceState)        setContentView(R.layout.activity_details)
        val txtDetails = findViewById<TextView>(R.id.txtDetails)        val btnBack = findViewById<Button>(R.id.btnBack)
        val items = intent.getStringArrayListExtra("items")        val categories = intent.getStringArrayListExtra("categories")        val quantities = intent.getIntegerArrayListExtra("quantities")        val comments = intent.getStringArrayListExtra("comments")
        var displayText = ""
        if (items != null) {
            for (i in items.indices) {
                displayText +=                    "Item: ${items[i]}\n" +                    "Category: ${categories!![i]}\n" +                    "Quantity: ${quantities!![i]}\n" +                    "Comments: ${comments!![i]}\n\n"            }        }
        txtDetails.text = displayText
        btnBack.setOnClickListener {            finish()        }    }}



On Wed, 10 Jun 2026, 13:27 Luyanda Ross, <dj.zeeno.rsa@gmail.com> wrote:
<?xml version="1.0" encoding="utf-8"?><ScrollView xmlns:android="http://schemas.android.com/apk/res/android"    android:layout_width="match_parent"    android:layout_height="match_parent"    android:background="#0F2027">
    <LinearLayout        android:layout_width="match_parent"        android:layout_height="wrap_content"        android:orientation="vertical"        android:padding="24dp">
        <TextView            android:layout_width="wrap_content"            android:layout_height="wrap_content"            android:text="Add New Gear"            android:textColor="#FFFFFF"            android:textSize="24sp"            android:textStyle="bold" />
        <EditText            android:id="@+id/editItemName"            android:layout_width="match_parent"            android:layout_height="wrap_content"            android:layout_marginTop="20dp"            android:hint="Item Name e.g. Tent"            android:textColor="#FFFFFF"            android:textColorHint="#888" />
        <EditText            android:id="@+id/editCategory"            android:layout_width="match_parent"            android:layout_height="wrap_content"            android:layout_marginTop="12dp"            android:hint="Category e.g. Shelter, Food, Safety"            android:textColor="#FFFFFF"            android:textColorHint="#888" />
        <EditText            android:id="@+id/editQuantity"            android:layout_width="match_parent"            android:layout_height="wrap_content"            android:layout_marginTop="12dp"            android:hint="Quantity"            android:inputType="number"            android:textColor="#FFFFFF"            android:textColorHint="#888" />
        <EditText            android:id="@+id/editComments"            android:layout_width="match_parent"            android:layout_height="wrap_content"            android:layout_marginTop="12dp"            android:hint="Comments e.g. 4-person waterproof"            android:textColor="#FFFFFF"            android:textColorHint="#888" />
        <Button            android:id="@+id/btnSaveGear"            android:layout_width="match_parent"            android:layout_height="wrap_content"            android:layout_marginTop="24dp"            android:backgroundTint="#2C5364"            android:text="Save Gear"            android:textColor="#FFFFFF" />
        <Button            android:id="@+id/btnBackToMain"            android:layout_width="match_parent"            android:layout_height="wrap_content"            android:layout_marginTop="8dp"            android:backgroundTint="#203A43"            android:text="Back to Base"            android:textColor="#FFFFFF" />
    </LinearLayout></ScrollView>

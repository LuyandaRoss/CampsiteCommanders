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
</activity>
<activity android:name=".MainActivity" />
<activity android:name=".AddGearActivity" />
<activity android:name=".DetailedViewActivity" />

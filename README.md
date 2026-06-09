# help
Please type in the text highlighted in red only:
1.MainActivity.kt
package com.example.travelpackingapp

import android.content.Intent
import android.os.Bundle
import android.util.Log
import android.widget.*
import androidx.appcompat.app.AppCompatActivity

class MainActivity : AppCompatActivity() {

    companion object {
        val itemArray = ArrayList<String>()
        val categoryArray = ArrayList<String>()
        val quantityArray = ArrayList<Int>()
        val commentsArray = ArrayList<String>()
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val edtItem = findViewById<EditText>(R.id.edtItem)
        val edtCategory = findViewById<EditText>(R.id.edtCategory)
        val edtQuantity = findViewById<EditText>(R.id.edtQuantity)
        val edtComments = findViewById<EditText>(R.id.edtComments)

        val btnAdd = findViewById<Button>(R.id.btnAdd)
        val btnSecond = findViewById<Button>(R.id.btnSecond)
        val btnExit = findViewById<Button>(R.id.btnExit)

        btnAdd.setOnClickListener {

            val item = edtItem.text.toString()
            val category = edtCategory.text.toString()
            val quantityText = edtQuantity.text.toString()
            val comments = edtComments.text.toString()

            // Error Handling
            if (item.isEmpty() || category.isEmpty() ||
                quantityText.isEmpty() || comments.isEmpty()) {

                Toast.makeText(this,
                    "Please fill in all fields",
                    Toast.LENGTH_SHORT).show()

                Log.e("INPUT_ERROR", "Empty fields detected")

            } else {

                try {
                    val quantity = quantityText.toInt()

                    itemArray.add(item)
                    categoryArray.add(category)
                    quantityArray.add(quantity)
                    commentsArray.add(comments)

                    Toast.makeText(this,
                        "Item Added Successfully",
                        Toast.LENGTH_SHORT).show()

                    Log.d("PACKING_APP", "Item Added: $item")

                    // Clear inputs
                    edtItem.text.clear()
                    edtCategory.text.clear()
                    edtQuantity.text.clear()
                    edtComments.text.clear()

                } catch (e: NumberFormatException) {

                    Toast.makeText(this,
                        "Quantity must be a number",
                        Toast.LENGTH_SHORT).show()

                    Log.e("INPUT_ERROR", "Invalid quantity")
                }
            }
        }

        btnSecond.setOnClickListener {
            val intent = Intent(this, SecondActivity::class.java)
            startActivity(intent)
        }

        btnExit.setOnClickListener {
            finishAffinity()
        }
    }
}







2. SecondActivity.kt

package com.example.travelpackingapp

import android.os.Bundle
import android.util.Log
import android.widget.Button
import android.widget.TextView
import androidx.appcompat.app.AppCompatActivity

class SecondActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_second)

        val btnDisplay = findViewById<Button>(R.id.btnDisplay)
        val btnQuantity = findViewById<Button>(R.id.btnQuantity)
        val btnBack = findViewById<Button>(R.id.btnBack)
        val txtOutput = findViewById<TextView>(R.id.txtOutput)

        btnDisplay.setOnClickListener {

            var output = ""

            for (i in MainActivity.itemArray.indices) {

                output += """
                    Item: ${MainActivity.itemArray[i]}
                    Category: ${MainActivity.categoryArray[i]}
                    Quantity: ${MainActivity.quantityArray[i]}
                    Comments: ${MainActivity.commentsArray[i]}

                """.trimIndent()

                output += "\n\n"
            }

            txtOutput.text = output

            Log.d("PACKING_APP", "Displayed full packing list")
        }

        btnQuantity.setOnClickListener {

            var output = ""

            for (i in MainActivity.quantityArray.indices) {

                if (MainActivity.quantityArray[i] >= 2) {

                    output +=
                        "${MainActivity.itemArray[i]} " +
                        "- Quantity: ${MainActivity.quantityArray[i]}\n"
                }
            }

            txtOutput.text = output

            Log.d("PACKING_APP", "Displayed quantity >= 2")
        }

        btnBack.setOnClickListener {
            finish()
        }
    }
}


Main Activity 

package com.example.klass_work_library

import android.os.Bundle
import androidx.appcompat.app.AppCompatActivity


class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val frag =
            supportFragmentManager.findFragmentById(R.id.detail_frag) as AnimalDetailFragment?
        frag!!.setAnimal(1)
    }
}

----------------------
AnimalListFragment

package com.example.klass_work_library

import android.R
import android.content.Context
import android.os.Bundle
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import android.widget.ArrayAdapter
import android.widget.ListView
import androidx.fragment.app.ListFragment


class AnimalListFragment : ListFragment() {
    override fun onCreateView(
        inflater: LayoutInflater,
        container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        val names = arrayOfNulls<String>(Animal.animals.size)
        for (i in names.indices) {
            names[i] = Animal.animals.get(i).getName()
        }
        val adapter = ArrayAdapter(inflater.context, R.layout.simple_list_item_1, names)
        listAdapter = adapter

        return super.onCreateView(inflater, container, savedInstanceState)
    }

    internal interface AnimalListListener {
        fun itemClicked(id: Long)
    }

    private var listener: AnimalListListener? = null

    override fun onAttach(context: Context) {
        super.onAttach(context)

        if (context is AnimalListListener) {
            listener = context
        }
    }

    override fun onListItemClick(l: ListView, v: View, position: Int, id: Long) {
        if (listener != null) {
            listener!!.itemClicked(id)
        }
    }
}

----------------------------
AnimalDetailFragment

package com.example.klass_work_library

import android.os.Bundle
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import android.widget.ImageView
import android.widget.TextView
import androidx.fragment.app.Fragment


class AnimalDetailFragment : Fragment() {
    private var animalId = 0

    override fun onCreateView(
        inflater: LayoutInflater,
        container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        return inflater.inflate(R.layout.fragment_animal_detail, container, false)
    }

    fun setAnimal(id: Int) {
        animalId = id
    }

    override fun onStart() {
        super.onStart()
        val view = view // ссылка на родительский макет фрагмента
        if (view != null) {
            val title =
                view.findViewById<TextView>(R.id.textTitle) // обращаться напрямую к вьюшкам фрагмент не может
            val animal: Animal = Animal.animals.get(animalId)
            title.setText(animal.getName())
            val description = view.findViewById<TextView>(R.id.textDescription)
            description.setText(animal.getDescription())
            val img = view.findViewById<ImageView>(R.id.picture)
            img.setImageResource(animal.getId())
        }
    }
}

----------------
Activity_main
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="horizontal">

    <androidx.fragment.app.FragmentContainerView
        android:id="@+id/list_frag"
        android:name="com.example.klass_work_library.AnimalListFragment"
        android:layout_width="0dp"
        android:layout_height="match_parent"
        android:layout_weight="2" />

    <androidx.fragment.app.FragmentContainerView
        android:id="@+id/detail_frag"
        android:name="com.example.klass_work_library.AnimalDetailFragment"
        android:layout_width="0dp"
        android:layout_height="match_parent"
        android:layout_weight="3" />

</LinearLayout>


-------------
fragmentAnimal_detail
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:orientation="vertical">

    <ImageView
        android:id="@+id/picture"
        android:layout_width="match_parent"
        android:layout_height="180dp"
        android:scaleType="centerCrop" />

    <TextView
        android:id="@+id/textTitle"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Название животного"
        android:textAppearance="?android:attr/textAppearanceLarge" />

    <TextView
        android:id="@+id/textDescription"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Описание животного" />

</LinearLayout>

---------------------------
fragmentAnimal_list

<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:orientation="vertical">

    <ImageView
        android:id="@+id/picture"
        android:layout_width="match_parent"
        android:layout_height="180dp"
        android:scaleType="centerCrop" />

    <TextView
        android:id="@+id/textTitle"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Название животного"
        android:textAppearance="?android:attr/textAppearanceLarge" />

    <TextView
        android:id="@+id/textDescription"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Описание животного" />

</LinearLayout>

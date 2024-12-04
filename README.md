# NotePad

## 一 功能扩展如下：

1:增强日志记录功能
2:引入笔记检索机制
3:实现笔记数据导出
4:优化笔记排序方式

## 二 主界面增加时间戳：

效果:

![1.png](images%2F1.png)
修改hello3:


![2.png](images%2F2.png)

1.:为了增强NoteList类的数据展示能力，我计划在其PROJECTION定义中新增一个时间戳字段。这样，当用户查看笔记列表时，每条笔记旁边都会附带一个清晰的时间戳，以便他们更好地追踪笔记的创建或更新时间。这一改动将提升用户界面的友好性和实用性。

```
private static final String[] PROJECTION = new String[] {
        NotePad.Notes._ID, // 0
        NotePad.Notes.COLUMN_NAME_TITLE, // 1
        NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE//显示时间戳

};
```



2.为了丰富NoteList类的数据显示和视图呈现，我计划在dataColumns数组中添加一个用于存储时间戳的字段，并在viewIDs映射中为这个新字段指定一个相应的视图标识符（view ID）。这样，当用户浏览笔记列表时，每条笔记都会伴随一个时间戳的显示，从而提供更加全面的信息展示。这一改进旨在提升用户体验和数据的可读性。

```
private String[] dataColumns = { NotePad.Notes.COLUMN_NAME_TITLE ,
        NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE } ;
private int[] viewIDs = { android.R.id.text1 , R.id.time };
```



3.在NotePadProvider类的insert方法中，为了确保日期数据以统一的格式存储，我计划添加日期格式转换的逻辑。这样，当用户通过该方法添加新笔记并包含日期信息时，系统会自动将该日期转换为指定的格式，以便于后续的存储、处理和显示。这一改动旨在提高数据的一致性和可管理性。

```
// 创建日期格式化对象，用于格式化当前日期
SimpleDateFormat dateFormat = new SimpleDateFormat("yyyy/MM/dd HH:mm:ss");

// 格式化当前日期为字符串
String formattedCurrentTime = dateFormat.format(currentDate);

// 检查值映射中是否包含创建日期字段，如果没有，则添加
if (!values.containsKey(NotePad.Notes.COLUMN_NAME_CREATE_DATE)) {
    values.put(NotePad.Notes.COLUMN_NAME_CREATE_DATE, formattedCurrentTime);
}

// 检查值映射中是否包含修改日期字段，如果没有，则添加
if (!values.containsKey(NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE)) {
    values.put(NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE, formattedCurrentTime);
}
```



4.在NodeEditor类的updateNote功能实现中，我计划引入一个步骤来转换日期数据的格式。具体来说，当用户通过该功能更新笔记并涉及到日期信息时，系统将会自动地、透明地将该日期转换为所需的格式。这一设计旨在确保日期数据的一致性和准确性，同时提升用户体验和数据处理的效率。

```
Long nowTime = Long.valueOf(System.currentTimeMillis());
java.sql.Date date = new Date(nowTime);
SimpleDateFormat format = new SimpleDateFormat("MM/dd/yyyy HH:mm:ss");
String Time = format.format(date);
ContentValues values = new ContentValues();
values.put(NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE, Time);
```



5.为了提升笔记列表界面的信息展示丰富度，我计划在list布局中新增一个用于显示时间戳的TextView组件。这样，用户在浏览笔记列表时，每条笔记旁边都会清晰地展示一个时间戳，帮助他们更好地追踪笔记的创建或更新时间。这一改动旨在增强用户界面的可读性和实用性。

```
<TextView
    android:id="@+id/time"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:textAppearance="?android:attr/textAppearance"
    android:paddingLeft="6dip"
    android:textColor="@color/black"/>
```



## 三、笔记搜索：

效果:

![3.png](images%2F3.png)
![4.png](images%2F4.png)
![5.png](images%2F5.png)

1.:为了提升用户界面的直观性和易用性，我计划在菜单配置文件中添加一个搜索功能的图标。这样，用户可以通过点击这个图标来快速启动搜索功能，从而更方便地查找和定位所需的笔记或信息。这一设计旨在优化用户体验，提高应用的交互性和便捷性。

```
<item
    android:id="@+id/menu_search"
    android:title="@string/menu_search"
    android:icon="@android:drawable/ic_search_category_default"
    android:showAsAction="always">
</item>
```



2.为了增强NoteList类的菜单选项响应能力，我计划在onOptionsItemSelected方法中扩展其功能，通过添加新的case分支来处理特定的菜单项点击事件。这样，当用户从菜单中选择某个选项时，NoteList类能够识别并响应这个选择，执行相应的操作或展示相应的界面。这一改动旨在提升用户界面的交互性和应用的灵活性。

```
case R.id.menu_search:
    Intent intent = new Intent();
    intent.setClass(NotesList.this,NoteSearch.class);
    NotesList.this.startActivity(intent);
    return true;
```

3.为了优化笔记搜索功能的用户界面，我计划创建一个新的布局文件note_search_list.xml。在这个布局文件中，我将设计并实现一个搜索框以及一个用于展示搜索结果的列表视图。这样，用户在进行笔记搜索时，可以方便地输入关键词，并即时查看到与搜索条件相匹配的笔记列表。这一设计旨在提升用户体验，使笔记搜索功能更加直观和高效。

```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="@color/white"> <!-- 设置整个布局的背景颜色为白色 -->

    <!-- 搜索框 -->
    <SearchView
        android:id="@+id/custom_search_view"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:iconifiedByDefault="false"
        android:background="#7A99D7"
        android:queryHint="搜索"
        android:layout_alignParentTop="true">
    </SearchView>

    <!-- ListView 组件 -->
    <ListView
        android:id="@android:id/custom_list"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:background="@color/white"> <!-- 设置ListView的背景颜色为白色 -->
    </ListView>

</LinearLayout>
```

4. 新建NoteSearch类:

```

public class NoteSearch extends ListActivity implements SearchView.OnQueryTextListener {
    private static final String[] PROJECTION = new String[] {
            NotePad.Notes._ID, NotePad.Notes.COLUMN_NAME_TITLE, NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE
    };
    private SearchView searchView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.note_search_list);
        Intent intent = getIntent();
        if (intent.getData() == null) {
            intent.setData(NotePad.Notes.CONTENT_URI);
        }
        searchView = (SearchView) findViewById(R.id.search_view);
        searchView.setOnQueryTextListener(this);
    }

    @Override
    public boolean onQueryTextSubmit(String query) {
        return false;
    }

    @Override
    public boolean onQueryTextChange(String newText) {
        updateNotesList(newText);
        return true;
    }

    private void updateNotesList(String newText) {
        String selection = NotePad.Notes.COLUMN_NAME_TITLE + " LIKE ?";
        String[] selectionArgs = { "%" + newText + "%" };//newText为搜索的内容
        Cursor cursor = managedQuery(
                getIntent().getData(),
                PROJECTION,
                selection,
                selectionArgs,
                NotePad.Notes.DEFAULT_SORT_ORDER
        );
        setListAdapter(new SimpleCursorAdapter(
                this,
                R.layout.noteslist_item,
                cursor,
                new String[]{NotePad.Notes.COLUMN_NAME_TITLE, NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE},
                new int[]{android.R.id.text1, R.id.time}
        ));
    }

    @Override
    protected void onListItemClick(ListView l, View v, int position, long id) {
        Uri uri = ContentUris.withAppendedId(getIntent().getData(), id);
        String action = getIntent().getAction();
        if (Intent.ACTION_PICK.equals(action) || Intent.ACTION_GET_CONTENT.equals(action)) {
            setResult(RESULT_OK, new Intent().setData(uri));
        } else {
            startActivity(new Intent(Intent.ACTION_EDIT, uri));
        }
    }
}
```

5.为了确保NoteSearch活动（Activity）能够被系统正确识别和启动，我计划在应用的AndroidManifest.xml文件中进行必要的注册配置。这样，当用户通过应用内的导航或外部链接尝试访问NoteSearch活动时，系统能够找到并加载这个活动的定义，从而展示相应的用户界面。这一步骤是Android应用开发中的标准流程，旨在确保所有活动都能够被正确地管理和调用:

```
<activity
    android:name="NoteSearch"
    android:label="@string/title_notes_search">
    </activity>
```

## 四、笔记导出：

![6.png](images%2F6.png)
![7.png](images%2F7.png)
![8.png](images%2F8.png)
1.添加导出按钮

```
<item android:id="@+id/menu_output"
    android:title="导出" />
```

2.为了提升用户从NoteEditor活动导出笔记内容的便捷性，我计划设计一个专门的方法，该方法负责从NoteEditor活动中触发并启动OutputText活动。这样，当用户完成笔记编辑并希望将内容导出时，可以通过点击相应的按钮或菜单项来调用这个方法，进而自动启动OutputText活动，展示导出的笔记内容。这一设计旨在优化用户操作流程，提高应用的可用性和用户满意度。

```
private final void outputNote() {
Intent intent = new Intent(null,mUri);
intent.setClass(NoteEditor.this,OutputText.class);
NoteEditor.this.startActivity(intent);
}
```

3.为了提供一个直观且用户友好的笔记导出界面，我计划设计和实现一个专门的布局。这个布局将包含所有必要的视图元素，如按钮、文本框等，以确保用户在导出笔记内容时能够清晰地看到相关信息，并方便地执行导出操作。通过精心设计和布局，我将致力于打造一个既美观又实用的导出界面，以提升用户的整体使用体验:

```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:orientation="vertical"
    android:padding="16dp">

    <EditText
        android:id="@+id/output_name"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginBottom="8dp"
        android:ems="10"
        android:hint="@string/enter_name"
        android:inputType="textCapSentences"
        android:maxLength="50" />

    <Button
        android:id="@+id/output_ok"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="@string/confirm"
        android:onClick="onConfirmClick" />
</LinearLayout>
```

4.新建OutputText类:

```
import android.app.Activity;
import android.database.Cursor;
import android.net.Uri;
import android.os.Bundle;
import android.view.View;
import android.widget.EditText;
import android.widget.Toast;

import java.io.File;
import java.io.FileOutputStream;
import java.io.OutputStreamWriter;
import java.io.PrintWriter;

public class TextOutputActivity extends Activity {
    private static final String[] DATABASE_COLUMNS = new String[]{
        NotePad.Notes._ID,
        NotePad.Notes.COLUMN_NAME_TITLE,
        NotePad.Notes.COLUMN_NAME_NOTE,
        NotePad.Notes.COLUMN_NAME_CREATE_DATE,
        NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE
    };
    private Cursor cursor;
    private EditText editTextTitle;
    private Uri noteUri;
    private boolean isReadyToWrite = false;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_text_output);
        noteUri = getIntent().getData();
        cursor = managedQuery(
            noteUri,
            DATABASE_COLUMNS,
            null,
            null,
            null
        );
        editTextTitle = findViewById(R.id.editTextTitle);
    }

    @Override
    protected void onResume() {
        super.onResume();
        if (cursor != null && !cursor.isClosed()) {
            cursor.moveToFirst();
            editTextTitle.setText(cursor.getString(1));
        }
    }

    @Override
    protected void onPause() {
        super.onPause();
        if (cursor != null && !cursor.isClosed() && isReadyToWrite) {
            saveText();
        }
        isReadyToWrite = false;
    }

    public void onConfirmClick(View view) {
        isReadyToWrite = true;
        finish();
    }

    private void saveText() {
        try {
            if (Environment.getExternalStorageState().equals(Environment.MEDIA_MOUNTED)) {
                String storagePath = Environment.getExternalStorageDirectory().getAbsolutePath();
                File outputFile = new File(storagePath + "/" + editTextTitle.getText().toString() + ".txt");
                PrintWriter writer = new PrintWriter(new OutputStreamWriter(new FileOutputStream(outputFile), "UTF-8"));
                writer.println(cursor.getString(1));
                writer.println(cursor.getString(2));
                writer.println("Created on: " + cursor.getString(3));
                writer.println("Last modified on: " + cursor.getString(4));
                writer.close();
                Toast.makeText(this, "Saved successfully, location: " + storagePath + "/" + editTextTitle.getText() + ".txt", Toast.LENGTH_LONG).show();
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

5.进行授权:

```xml
<!-- 向SD卡写入数据权限 -->
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
<!-- 读取SD卡数据权限 -->
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>
```

## 五 、笔记排序：

![9.png](images%2F9.png)

按创建时间排序:

![10.png](images%2F10.png)

按修改时间排序:

![11.png](images%2F11.png)
![12.png](images%2F12.png)

1.为了丰富应用的用户界面并提升用户体验，我计划在应用的菜单中添加一些新的选项。这些选项将为用户提供更多样的功能和操作选择，使他们能够更便捷地完成各项任务。通过精心设计和布局，我将致力于打造一个直观、易用且功能丰富的菜单界面:

```
<item
    android:id="@+id/menu_sort1"
    android:title="按创建时间排序"/>
<item
    android:id="@+id/menu_sort2"
    android:title="按修改时间排序"/>
```

2.为了增强notesList的用户交互性，我计划在notesList的菜单中添加几个新的响应选项（即菜单case）。这些选项将为用户提供额外的操作选择，使他们能够更灵活地管理笔记列表。通过实现这些菜单case，我将确保notesList的用户界面既直观又功能丰富，从而满足用户的多样化需求:

```

//创建时间排序

case R.id.menu_sort1:
     cursor = managedQuery(
            getIntent().getData(),
            PROJECTION,
            null,
            null,
            NotePad.Notes._ID
    );
     adapter = new SimpleCursorAdapter(
            this,
            R.layout.noteslist_item,
            cursor,
            dataColumns,
            viewIDs
    );
    setListAdapter(adapter);
    return true;

//修改时间排序

case R.id.menu_sort2:
    cursor = managedQuery(
            getIntent().getData(),
            PROJECTION,
            null,
            null,
            NotePad.Notes.DEFAULT_SORT_ORDER
    );
    adapter = new SimpleCursorAdapter(
            this,
            R.layout.noteslist_item,
            cursor,
            dataColumns,
            viewIDs
    );
    setListAdapter(adapter);
    return true;
```


<p align="center">
  <img src="logo.png" width=400 />
</p>

[ ![Download](https://api.bintray.com/packages/heckslam/EditorJSKit/ejkit/images/download.svg) ](https://bintray.com/heckslam/EditorJSKit/ejkit/_latestVersion)

## About

A non-official Android Framework for [Editor.js](https://editorjs.io) - block styled editor. It's purpose to make easy use of rendering and parsing of blocks.

Converts clean json blocks data like [this](app/src/main/assets/dummy_data.json) into native views like that 👇

<p align="center">
  <img src="screenshot.png" width=420 />
</p>

#### Supported blocks
* 🎩 Header
* 🥑 Raw HTML
* 📷 Image
* 🖌 Delimiter
* 💌 Paragraph
* 🕸 Link
* 🌿 List
* 📋 Table

## Installation

```
maven { url "https://dl.bintray.com/heckslam/EditorJSKit" }
compile 'com.github.upstarts:ejkit:X.X.X' - look at badge above for latest version
```

## Setup
 1. Create adapter instance in your activity/fragment (Style parameter is optional)
 ```
    private val rvAdapter: EditorJsAdapter by lazy {
        EditorJsAdapter(EJStyle.create(this.applicationContext))
    }
```
 2. Set adapter to recyclerview in your layout
 3. Init gson with custom Deserializer
```
    val blocksType = object : TypeToken<MutableList<EJBlock>>() {}.type
    val gson = GsonBuilder()
        .registerTypeAdapter(blocksType, EJDeserializer())
        .create()
```
 4. Apply received data to adapter
```
    rvAdapter.items = ejResponse.blocks
```

## Customizing
You can set style globally via `EJKit.style = ...` for all adapters, or pass `EJStyle` instance for specific adapter.
```
EJKit.ejStyle = EJStyle.emptyBuilder()
            .bulletDrawableRes(R.drawable.list_circle) // used for unordered list
            .tableColumnDrawableRes(R.drawable.table_content_cell_bg) // table column background
            .imageBackgroundRes(R.drawable.image_background) // background for Image block
            .linkColor(getColor(R.color.colorPrimary))
            .blockMargin(resources.getDimensionPixelSize(R.dimen.padding_small)) //used as padding for Paragraph and Raw Html blocks
            .listItemColor(getColor(R.color.colorPrimary))
            .listBulletColor(getColor(R.color.colorPrimary))
            .imageBorderRes(R.drawable.image_background)
            .tableColumnTextColor(getColor(R.color.colorPrimary))
            .paragraphTextColor(getColor(R.color.colorPrimary))
            .paragraphBackgroundColor(getColor(R.color.colorPrimary))
            .paragraphTypeface(ResourcesCompat.getFont(this, R.font.alice)!!)
            .paragraphTextSize(resources.getDimensionPixelSize(R.dimen.default_font_size))
            .headingTypeface(ResourcesCompat.getFont(this, R.font.alice)!!)
            .headingTextSizes(floatArrayOf(32f, 24f, 18f, 16f, 14f, 12f)) // sizes used by default for headers
            .thematicBreakColor(getColor(R.color.colorPrimary)) // delimeter color
            .thematicBreakHeight(resources.getDimensionPixelSize(R.dimen.delimiter_height)) //delimiter height
            .build()
```

### Override android style in styles.xml
```
<style name="HeaderText" parent="@android:style/TextAppearance">
    <item name="android:textColor">@color/colorPrimary</item>
</style>
<style name="ParagraphText" parent="@android:style/TextAppearance">
    <item name="android:textSize">16sp</item>
</style>
```

### Create custom block
If you have block that library does not provide currently, you can add it by yourself.
1. Create custom block type and extend it from `EJAbstractBlockType`
```
enum class CustomBlockType(override val jsonName: String) : EJAbstractBlockType {
    TABLE("table")
}
```
2. Create data class for block
```
data class EJTableData(
    val content: List<List<String>>
): EJData()
```
3. And register it
`EJKit.register(EJCustomBlock(CustomB.TABLE, EJTableData::class.java))
`

## Example

You can find and test the example in a [sample activity](app/src/main/java/work/upstarts/MainActivity.kt)

## Author

[Upstarts team](https://upstarts.work)

[Vadim Popov](https://t.me/popovvadim) - Architecture draft

[Ruslan Aliev](https://github.com/heckslam) - Architecture, Implementation
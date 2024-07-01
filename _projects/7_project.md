---
layout: page
title: project 7
description: with background image
img: assets/img/4.jpg
importance: 1
category: work
related_publications: true
---

Every project has a beautiful feature showcase page.
It's easy to include images in a flexible 3-column grid format.
Make your photos 1/3, 2/3, or full width.

To give your project a background in the portfolio page, just add the img tag to the front matter like so:

    ---
    layout: page
    title: project
    description: a project with a background image
    img: /assets/img/12.jpg
    ---

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/1.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/3.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/5.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Caption photos easily. On the left, a road goes through a tunnel. Middle, leaves artistically fall in a hipster photoshoot. Right, in another hipster photoshoot, a lumberjack grasps a handful of pine needles.
</div>
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/5.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    This image can also have a caption. It's like magic.
</div>

You can also put regular text between your rows of images, even citations {% cite einstein1950meaning %}.
Say you wanted to write a bit about your project before you posted the rest of the images.
You describe how you toiled, sweated, _bled_ for your project, and then... you reveal its glory in the next row of images.

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/6.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/11.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    You can also have artistically styled 2/3 + 1/3 images, like these.
</div>

The code is simple.
Just wrap your images with `<div class="col-sm">` and place them inside `<div class="row">` (read more about the <a href="https://getbootstrap.com/docs/4.4/layout/grid/">Bootstrap Grid</a> system).
To make images responsive, add `img-fluid` class to each; for rounded corners and shadows use `rounded` and `z-depth-1` classes.
Here's the code for the last row of images above:

{% raw %}

```html
<div class="row justify-content-sm-center">
  <div class="col-sm-8 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/6.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
  </div>
  <div class="col-sm-4 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/11.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
  </div>
</div>
```

```java
import static org.junit.Assert.assertNotNull;
import static org.junit.Assert.assertTrue;

public class TestValidatedCSV {
    private static final Logger LOGGER = LoggerFactory.getLogger(TestValidatedCSV.class);

    @Test
    public void testCatalogCSVValidateByFilePath() {
        String targetOutput = new File(Objects.requireNonNull(this.getClass().getResource("/")).getPath()).getAbsolutePath();
        String catalogDir = StringUtils.join(Arrays.asList(targetOutput, "catalog"), File.separator);
        String[] arg = new String[]{catalogDir};
        try {
            CSVClLoader.main(arg);
        } catch (Exception e) {
            LOGGER.error("CSVClLoader throw exception: {}", e);
        }
    }

    @Test
    public void testAssembleCatalogAttribute() {
        String targetOutput = new File(Objects.requireNonNull(this.getClass().getResource("/")).getPath()).getAbsolutePath();
        String catalogDir = StringUtils.join(Arrays.asList(targetOutput, "catalog"), File.separator);
        String[] arg = new String[]{catalogDir};
        List<CatalogAttribute> catalogAttributes = new ArrayList<>();
        try {
            CSVClLoader loader = new CSVClLoader();
            loader.run(arg);
            List<CatalogItemImpl> catalogItems = loader.getCatalogItems();

            catalogItems.forEach(catalogItem -> {
                CatalogAttribute catalogAttribute = new CatalogAttribute();
                extractCatalogAttributes(catalogItem, catalogAttribute);
                catalogAttributes.add(catalogAttribute);
            });

            assertNotNull("catalogItems should not be null", catalogItems);
            assertTrue("catalogItems should not be empty", !catalogItems.isEmpty());

            String json = convertCatalogAttributeToJson(catalogAttributes);
            writeJsonToFile(json, "catalogAttributes.json");

        } catch (Exception e) {
            LOGGER.error("CSVClLoader throw exception: {}", e);
        }
    }

    private String convertCatalogAttributeToJson(List<CatalogAttribute> catalogAttributes) {
        String json = "";
        try {
            json = new ObjectMapper().writeValueAsString(catalogAttributes);
        } catch (JsonProcessingException e) {
            LOGGER.error("Failed to convert catalogAttributes to json: {}", e);
        }
        return json;
    }

    private void writeJsonToFile(String json, String fileName) {
        try {
            File file = new File(fileName);
            new ObjectMapper().writeValue(file, json);
        } catch (IOException e) {
            LOGGER.error("Failed to write json to file: {}", e);
        }
    }

    private void extractCatalogAttributes(CatalogItemImpl catalogItem, CatalogAttribute catalogAttribute) {
        Boolean cde = catalogItem.isCde();
        catalogAttribute.setCde(cde);
        String catalogItemDescription = catalogItem.getDescription();
        catalogAttribute.setDescription(catalogItemDescription);
        String catalogItemName = catalogItem.getName();
        catalogAttribute.setAttributeName(catalogItemName);
        Boolean derived = catalogItem.isDerived();
        catalogAttribute.setDerived(derived);
        catalogItem.getPhysicalItems().forEach(physicalItem -> {
            String physicalTableName = physicalItem.getPhysicalTable().getName();
            catalogAttribute.setPhysicalTableName(physicalTableName);
        });
    }
}
```

{% endraw %}

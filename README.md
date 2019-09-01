# GraphToDTreeConverter

This module converts a given graph data structure to the data format that can be used by the dTree visualizing library from ErikGartner (https://github.com/ErikGartner/dTree).

To visualize a family tree with the dTree library you have to provide the data in a hierarchical format:
```
Parent1:
    Marriage:
        Parent2
    Childs...
        Child1
        Child2
            Marriage:
                PersonX
            Childs:
                ...
        ...
```

With this data format it's not possible to save multiple family trees. Also it's only possible to render one parent history with the dTree library.
But i wanted to have the possiblity to *save* multiple family trees (for ``Parent1`` and ``Parent2``) in one data structure. The rendering of multiple family trees wasn't important to me.

So I created a (graph) data structure that can be converted to the necessary dTree format.

## Example graph data structure:
``FamilyGraphData``
```
{
  "persons": [
    {
      "id": 0,
      "name": "Parent1",
      "gender": "man"
    },
    {
      "id": 1,
      "name": "Parent2",
      "gender": "woman"
    },
    {
      "id": 2,
      "name": "Child1",
      "gender": "woman"
    },
    {
      "id": 3,
      "name": "Child2",
      "gender": "man"
    }
  ],
  "connections": [
    {
      "partner1Id": 0,
      "partner2Id": 1,
      "childrenIds": [2, 3]
    }
  ]
}
```
It's important to note, that the first person has the id 0 and that there is no gap between the ids.

## Usage of the converter
```js
const GraphToDTreeConverter = require("graphtodtreeconverter");

var familyRelations = GraphToDTreeConverter(
        FamilyGraphData,
        rootId);
```
``FamilyGraphData``: The graph data structure

``rootId``: The id of the person which family tree should be generated

If the given person (``rootId``) has additional parent informations, the root of the family tree will be changed to the "first" parent.

### Example: 
The call of ``GraphToDTreeConverter(FamilyGraphData, 2)`` will result in:
```
[
  {
    name: 'Parent1',
    class: 'man',
    extra: {
      id: 0
    },
    marriages: [
      {
        spouse: {
          name: 'Parent2',
          class: 'woman',
          extra: {
            id: 1
          }
        },
        children: [
          {
            name: 'Child1',
            class: 'woman',
            extra: {
              id: 2
            }
          },
          {
            name: 'Child2',
            class: 'man',
            extra: {
              id: 3
            }
          }
        ]
      }
    ]
  }
]
```






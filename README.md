# Automating-Nut-Meshing-in-Ansys-Mechanical
 Automating Nut Meshing in Ansys Mechanical – Saving Time & Standardizing Mesh Quality!
 I recently developed a custom "Nut" button in Ansys Mechanical that automates the entire nut meshing process with just a single click!

## Short description
🔹 What does it do?
 ✅ Automatically detects and classifies nuts based on size
 ✅ Applies standardized mesh settings for consistency
 ✅ Saves significant pre-processing time by eliminating manual meshing steps
 
### What's the problem?

Regular ways are time consuming and no standadization in mesh.

### How can technology help?
With the use of pyprimemesh, pymechanical linraries of pyansys, we can automate the process and saves our pre-processing time.

### Source
[nut mesh.txt](https://github.com/user-attachments/files/19408837/nut.mesh.txt)
![nut1](https://github.com/user-attachments/assets/c85906d1-7105-4e52-bc68-bbe296519b37)

## Acknowledgments / Reference Links

https://developer.ansys.com/docs
https://mechanical.docs.pyansys.com/
https://prime.docs.pyansys.com/version/stable/

Here is some short code for reference

## code

def createNut(analysis):
    """
    The method is called when the toolbar button is clicked.
    Keyword arguments:
    analysis -- the active analysis
    """

    # Add the "Nut" ACT load as defined in the XML file in the selected analysis tree.
    analysis.CreateLoadObject("Nut", ExtAPI.ExtensionManager.CurrentExtension)

#named selection
#Step 1 create named selection by selecting all nuts and rename it as Nut
#Creating small and big nut named selection

sel = Model.AddNamedSelection()
sel.ScopingMethod = GeometryDefineByType.Worksheet
sel.Name = "smallnut"
Nut = sel.GenerationCriteria
Nut.Add(None)
Nut[0].EntityType = SelectionType.GeoBody
Nut[0].Criterion = SelectionCriterionType.NamedSelection
Nut[0].Operator = SelectionOperatorType.Equal
Nut[0].Value = DataModel.GetObjectsByName("Nut")[0]

sel.Generate()

sel = Model.AddNamedSelection()
sel.ScopingMethod = GeometryDefineByType.Worksheet
sel.Name = "bignut"
Nut = sel.GenerationCriteria
Nut.Add(None)
Nut[0].EntityType = SelectionType.GeoBody
Nut[0].Criterion = SelectionCriterionType.NamedSelection
Nut[0].Operator = SelectionOperatorType.Equal
Nut[0].Value = DataModel.GetObjectsByName("Nut")[0]

Nut.Add(None)
Nut[1].Action = SelectionActionType.Filter
Nut[1].EntityType = SelectionType.GeoBody
Nut[1].Criterion = SelectionCriterionType.Size
Nut[1].Operator = SelectionOperatorType.GreaterThan
Nut[1].Value = Quantity("1600 [mm mm mm]")

sel.Generate()

#Creating Nutface named selection

sel = Model.AddNamedSelection()
sel.ScopingMethod = GeometryDefineByType.Worksheet
sel.Name = "smallnutface"
Nut = sel.GenerationCriteria
Nut.Add(None)
Nut[0].EntityType = SelectionType.GeoBody
Nut[0].Criterion = SelectionCriterionType.NamedSelection
Nut[0].Operator = SelectionOperatorType.Equal
Nut[0].Value = DataModel.GetObjectsByName("smallnut")[0]

Nut.Add(None)
Nut[1].Action = SelectionActionType.Convert
Nut[1].EntityType = SelectionType.GeoFace

Nut.Add(None)
Nut[2].Action = SelectionActionType.Filter
Nut[2].EntityType = SelectionType.GeoFace
Nut[2].Criterion = SelectionCriterionType.Type
Nut[2].Operator = SelectionOperatorType.Equal
Nut[2].Value = 2
sel.Generate()

sel = Model.AddNamedSelection()
sel.ScopingMethod = GeometryDefineByType.Worksheet
sel.Name = "bignutface"
Nut = sel.GenerationCriteria
Nut.Add(None)
Nut[0].EntityType = SelectionType.GeoBody
Nut[0].Criterion = SelectionCriterionType.NamedSelection
Nut[0].Operator = SelectionOperatorType.Equal
Nut[0].Value = DataModel.GetObjectsByName("bignut")[0]

Nut.Add(None)
Nut[1].Action = SelectionActionType.Convert
Nut[1].EntityType = SelectionType.GeoFace

Nut.Add(None)
Nut[2].Action = SelectionActionType.Filter
Nut[2].EntityType = SelectionType.GeoFace
Nut[2].Criterion = SelectionCriterionType.Type
Nut[2].Operator = SelectionOperatorType.Equal
Nut[2].Value = 2
sel.Generate()
sel = Model.AddNamedSelection()
sel.ScopingMethod = GeometryDefineByType.Worksheet
sel.Name = "smallnutface1"
Nut = sel.GenerationCriteria
Nut.Add(None)
Nut[0].EntityType = SelectionType.GeoBody
Nut[0].Criterion = SelectionCriterionType.NamedSelection
Nut[0].Operator = SelectionOperatorType.Equal
Nut[0].Value = DataModel.GetObjectsByName("smallnut")[0]

#Creating Nutedge named selection

sel = Model.AddNamedSelection()
sel.ScopingMethod = GeometryDefineByType.Worksheet
sel.Name = "smallnutedge"
Nut = sel.GenerationCriteria
Nut.Add(None)
Nut[0].EntityType = SelectionType.GeoBody
Nut[0].Criterion = SelectionCriterionType.NamedSelection
Nut[0].Operator = SelectionOperatorType.Equal
Nut[0].Value = DataModel.GetObjectsByName("smallnut")[0]

Nut.Add(None)
Nut[1].Action = SelectionActionType.Convert
Nut[1].EntityType = SelectionType.GeoEdge


sel = Model.AddNamedSelection()
sel.ScopingMethod = GeometryDefineByType.Worksheet
sel.Name = "bignutedge2"
Nut = sel.GenerationCriteria
Nut.Add(None)
Nut[0].EntityType = SelectionType.GeoBody
Nut[0].Criterion = SelectionCriterionType.NamedSelection
Nut[0].Operator = SelectionOperatorType.Equal
Nut[0].Value = DataModel.GetObjectsByName("bignut")[0]

Nut.Add(None)
Nut[1].Action = SelectionActionType.Convert
Nut[1].EntityType = SelectionType.GeoEdge

Nut.Add(None)
Nut[2].Action = SelectionActionType.Filter
Nut[2].EntityType = SelectionType.GeoEdge
Nut[2].Criterion = SelectionCriterionType.Type
Nut[2].Operator = SelectionOperatorType.Equal
Nut[2].Value = 1
sel.Generate()

#Mesh

mesh = Model.Mesh

method = mesh.AddAutomaticMethod()
Nut = DataModel.GetObjectsByName("Nut")[0]
method.Location = Nut
method.Method = MethodType.AllTriAllTet
if Nut.TotalSelection == 0 :
    method.Suppressed = True
else:
    method.Suppressed = False


SIZING = mesh.AddSizing()
bignutface = DataModel.GetObjectsByName("bignutface")[0]
SIZING.Location = bignutface
SIZING.ElementSize = Quantity('3 [mm]')
if bignutface.TotalSelection == 0 :
    SIZING.Suppressed = True
else:
    SIZING.Suppressed = False

smallnutface = DataModel.GetObjectsByName("smallnutface")[0]
facemesh = mesh.AddFaceMeshing()
facemesh.Location = smallnutface
if smallnutface.TotalSelection == 0 :
    facemesh.Suppressed = True
else:
    facemesh.Suppressed = False

bignutface = DataModel.GetObjectsByName("bignutface")[0]
facemesh = mesh.AddFaceMeshing()
facemesh.Location = bignutface
if bignutface.TotalSelection == 0 :
    facemesh.Suppressed = True
else:
    facemesh.Suppressed = False

smallnutface1 = DataModel.GetObjectsByName("smallnutface1")[0]
facemesh = mesh.AddFaceMeshing()
facemesh.Location = smallnutface1
if smallnutface1.TotalSelection == 0 :
    facemesh.Suppressed = True
else:
    facemesh.Suppressed = False

bignutface1 = DataModel.GetObjectsByName("bignutface1")[0]
facemesh = mesh.AddFaceMeshing()
facemesh.Location = bignutface1
if bignutface1.TotalSelection == 0 :
    facemesh.Suppressed = True
else:
    facemesh.Suppressed = False

mws = Model.Mesh.Worksheet
nsels = Model.NamedSelections
Nut = nsels.Children[0]
mws.AddRow()
mws.SetNamedSelection(0,Nut)
mws.SetActiveState(0,True)
mws.GenerateMesh()


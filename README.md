# 41934 - Advanced BIM - Group 36

## Introduction
The goal is to support the user in optimizing the building and reduce the use of material through FEM analysis. In this assigment there has been made a phyton script that make a new propertySet and inserting properties in a IFC file such as the moment of inertia and Young's modulus of elasticity. These new properties are ofc not enough we also need Support and actuel calculation, this will be futher be descibed in the report.

### Group members
Harald Lyngbye (s203615)  
Thomas Lyskjær (s203626)  
Kasper Bengtsson (s203628)



# 3A: Analyse use case
## 1
The goal of the tool can bee seen in the BIM execution plan under goal.
[Link to the goal section](BIM_ExecutionPlan.md#Goal)

## 2
the model Use  can bee seen in the BIM execution plan under model.
[Link to the model section](BIM_ExecutionPlan.md#Model)

# 3B: Propose a (design for a) tool / workflow
## 1
The IDM diagram can bee seen in BIM execution plan under process.
[Link to the Process section](BIM_ExecutionPlan.md#process)

## 2
The description of the process of your tool / workflow can also bee seen in the BIM execution plan under process
[Link to the Process section](BIM_ExecutionPlan.md#Description)


# 3D: Value What is the potential improvement offered by this tool? 

## Cost Reduction

The tool is projected to significantly reduce material costs, offering a direct financial benefit. By refining material requirements, the tool also potentially lowers secondary expenses related to logistics and handling.

## Efficiency and Productivity

Automating the FEM analysis and integrating it with BIM workflows through IFC compatibility is expected to accelerate the design process. This increment in productivity could enhance employee job satisfaction and allow businesses to reallocate resources to innovation-driven tasks.

## Competitive Edge

The capacity to deliver optimized structural designs may provide a significant competitive advantage in the market, promoting the business as a pioneer in smart construction methods.

## Risk Management

The advanced analysis capabilities of the tool will likely facilitate early detection of structural design issues, mitigating risks associated with construction errors and subsequent liability.

## Environmental Management

Material optimization directly contributes to environmental conservation by decreasing the demand for raw materials, thereby reducing the environmental footprint of construction projects.

# IFC models

The model used and the modified model are uploaded on learn due to them beeing a skylab model.

# Phyton script 
The Python script created for this assignment is designed to enhance an IFC file by inserting structural parameters such as the moment of inertia and Young's modulus of elasticity. This is achieved by creating a new PropertySet and inserting properties. For this task, a new PropertySet has been associated with a specific beam element named "Rectangular and Square Hollow Sections:SB10:2518553," bearing the global ID: 0PBox$QKjEKQwoUJvZkwUp.
## Picture of the new PropertySet in blender
![Alt text](images/property_set.JPG)


## Phyton code


```python
import ifcopenshell

# Configuration Parameters
ifc_file_path = 'C:/Users/kaspervang/Desktop/overgangs semester/Advance BIM/A2/LLYN - STRU.ifc'
output_ifc_file_path = 'C:/Users/kaspervang/Desktop/overgangs semester/Advance BIM/A3/LLYN - STRU_Modified.ifc'
global_id = '0PBox$QKjEKQwoUJvZkwUp'
property_set_name = 'Pset_FEMAnalysis'
properties_to_update = {
    'Moment of inertia, M_y': 1370000,
    'Moment of inertia, M_z': 1370000,
    'Youngs modulus, E': 2100000000,
}

# Load the IFC file
print("Loading IFC file...")
ifc_file = ifcopenshell.open(ifc_file_path)

# Get the element by GlobalId
print(f"Getting element by GlobalId: {global_id}")
element = ifc_file.by_guid(global_id)
if not element:
    print("Element not found. Exiting.")
    exit()

print(f"Element found: {element}")

# Function to get or create property set
def get_or_create_pset(element, pset_name):
    print(f"Getting or creating property set: {pset_name}")
    for definition in element.IsDefinedBy:
        if definition.is_a('IfcRelDefinesByProperties'):
            property_set = definition.RelatingPropertyDefinition
            if property_set.Name == pset_name:
                print(f"Found existing property set: {pset_name}")
                return property_set
    # Property set not found, create a new one
    print(f"Creating new property set: {pset_name}")
    property_set = ifc_file.createIfcPropertySet(
        GlobalId=ifcopenshell.guid.new(),
        OwnerHistory=ifc_file.by_type('IfcOwnerHistory')[0],
        Name=pset_name,
        Description=None,
        HasProperties=[]
    )
    ifc_file.createIfcRelDefinesByProperties(
        GlobalId=ifcopenshell.guid.new(),
        OwnerHistory=ifc_file.by_type('IfcOwnerHistory')[0],
        RelatedObjects=[element],
        RelatingPropertyDefinition=property_set
    )
    return property_set

# Try to add or update property
try:
    property_set = get_or_create_pset(element, property_set_name)

    for property_name, property_value in properties_to_update.items():
        print(f"Checking for existing property: {property_name}")
        existing_property = next(
            (prop for prop in property_set.HasProperties if prop.Name == property_name),
            None
        )

        if existing_property:
            # If the property exists, update its value
            print(f"Updating existing property: {property_name}")
            existing_property.NominalValue.wrappedValue = property_value
        else:
            # Create a new property and add it to the property set
            print(f"Creating new property: {property_name}")
            nominal_value = ifc_file.createIfcReal(property_value)
            new_property = ifc_file.createIfcPropertySingleValue(
                Name=property_name,
                Description=None,
                NominalValue=nominal_value,
                Unit=None  # Set to None or create IfcMonetaryUnit if necessary
            )
            properties_list = list(property_set.HasProperties) if isinstance(property_set.HasProperties, tuple) else property_set.HasProperties
            properties_list.append(new_property)
            property_set.HasProperties = properties_list

    # Save the IFC file with the new properties
    print(f"Saving IFC file to: {output_ifc_file_path}")
    ifc_file.write(output_ifc_file_path)
    print("IFC file saved successfully.")

except Exception as general_exception:
    print(f"An error occurred: {general_exception}")







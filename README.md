# Neos Best Practices

The recommendation we are giving here do not mean that things are wrong if done differently, there are valid reasons to deviate from those guidelines. However with no specific reasons speaking against we strongly recommend to obey to the following rules.

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED",  "MAY", and "OPTIONAL" in this document are to be interpreted as described in RFC 2119.

Note: These rules will eventually end up in the Neos-documentation where they will be reevaluated and extended in regular intervals. We will use semantic versioning for these best practises.

## NodeTypes

1. Each NodeType SHOULD be defined in a dedicated yaml-file and the file-name MUST represent the namespace of the contained NodeType/s. This helps finding the definition of a node type and get an understanding of the existing NodeTypes by looking at the configuration folder.

1. The namespace of the your own NodeTypes SHOULD be structured and nested. It is RECOMMEND to start with one of the prefixes "Document", "Content", "Mixin", "Collection" or "Constraint".

  ​​"Document" NodeTypes inherit from Neos.Neos:Document
  "Content" NodeTypes inherit from Neos.Neos:Content
  "Collection" NodeTypes which inherit from Neos.Neos:ContentCollections
  "Mixin" NodeTypes are abstract and define a reusable set of properties or child nodes.
  "Constraint" NodeTypes are abstract and define the allowed usage of Nodes.

1. NodeType-files that modify NodeTypes from other Packages MUST have “Override” in the filename.

1. Sub-NodeTypes that are bound to a specific parent NodeType SHOULD have a name matching the parent, e.g. "Vendor:Content.Slider" and "Vendor:Content.Slider.Item"

1. Properties SHOULD only be editable by a single editor – either inline or in the inspector. Editing the same property with different editors like inspector and inline easily causes problems when settings are only slightly off.

1. Each NodeType property MUST be valid after creating a new node. This can be done by defining the defaultValue or by allowing the property being empty. Especially SelectBoxEditors caused trouble in the past if they neither allowed being empty nor had good defaults.

1. The Neos.Neos.NodeTypes package SHOULD NOT be used directly. We RECOMMEND to create NodeTypes in the project namespace. You MAY use the Mixins from Neos.Neos.NodeTypes.BaseMixins instead.

1. A NodeType SHOULD only inherit from a single non-abstract superType. All other superTypes SHOULD be Mixins, or Constraints. This helps keeping the inheritance chain understandable.

## Fusion

1. The Root.Fusion in a project MUST not contain anything but includes.

1. Fusion files SHOULD only contain a single prototype. The folder and filenames MUST reflect the names of the contained prototypes.

1. The Fusion prototype-namespace SHOULD be structured and nested. It is RECOMMENDED to start with one of the prefixes “Content”, “Document”, “Component”,  “Helper” “Presentation” and “Integration”.

1. Each non-abstract NodeType SHOULD have a matching Fusion prototype. This is the default way of Neos to find a matching Renderer and makes the code easy to understand. We RECOMMEND to abstract code into Fusion prototypes that are not bound to NodeTypes and reuse them across the project.

1. Fusion prototypes with the prefixes “Content” and “Document” SHOULD implement the rendering of a Content or Document NodeType.

1. Fusion prototypes with the “Component” Prefix SHOULD implement reusable rendering aspects.

1. The rendering of content SHOULD be defined in Fusion-AFX. Fluid-Templates are still supported but no longer considered best-practice.

1. Fusion-files that modify prototypes from other packages MUST have “Override” in the file- or folder name.

1. For larger projects the presentainal prototypes SHOULD be separated from Integration. This can be done with seperate packages or folders “Presentation”, “Integration”. Those presentational Fusion prototypes MUST implement rendering aspects only without fetching data from the ContentRepository. 

1. Visual differentiations between Documents SHOULD be implemented via separate Document-NodeTypes. Especially the layout property from Neos.Neos:NodeTypes.Page layout property SHOULD NOT be used.

## Distributions

1. Neos projects SHOULD be managed as a single git repository that contains all packages that are specific to this project. This gives you a shared git history for all project-specific code.

1. The “Packages” folder SHOULD only be controlled by composer. All your own project-specific packages MUST be be stored in special directory like “DistributionPackages” that is referenced as a repository of type path in the main composer file.

## Public Packages

1. We RECOMMEND to separate reusable Mixins, Helpers and Prototypes from concrete NodeTypes, Configurations and Rendering. This makes it easy to only use specific parts of your package.
You MAY provide presentational components without a direct coupling to the NodeTypes. This makes your package even more flexible other developers.

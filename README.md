# OSM Project

The main repository for the route planer project. The different components are managed in their own Git-repositories. See the list below for details.

## Components
- [Data](https://github.com/HSNR-OSM-2018/Data): Basic structures used by various components. The repository contains the classes that build the underlying graph.
- [Import](https://github.com/HSNR-OSM-2018/Import): Used to build the PBF-files with custom structure based on OpenStreetMap exports. During the import irrelevant information is deleted and the structure is adjusted to fit the projects needs (independent component).
- [Pathfinder](https://github.com/HSNR-OSM-2018/route-planner-core): Implementation of numerous path-finding algorithms that operate on the graph defined by the Data component.
- [Provider](https://github.com/HSNR-OSM-2018/Provider): Reading the custom PBF-files and building the graph defined in the Data component.
- [Server](https://github.com/HSNR-OSM-2018/Server): Interface for the user built based on an HTTP-Server. The frontend provides an OSM interface and inputs for searching paths. The backend builds and manages the graph at creations and calls path-finding algorithms when the user searches for a path.

## Installation

#### Requirements
The project requires Java and maven.

#### Download repositories
Clone / download this repository. Then clone / download the components listed above into subfolders. The names of the subfolders are the same as the components except for the pathfinder (download this in folder named 'route-planner-core').

#### Build
Build the project by running `mvn clean package`. The most important outputs are two jar-files containing the importer and the server. Their usage is explained below.

# Usage

#### Importer
The Importer jar will be generated in `Import/target/osm-import.jar`. Before running it you need to copy the jar and both Shell-scripts found in `Import/src/main/ressources/` into an folder where the source pbf file is located.
Then run the import with `java -jar osm-import.jar <pbffile>`. For large files more RAM is required, this can be specified by extra arguments like `-ea -Xmx16g -Xms16g -XX:-UseGCOverheadLimit`.
The importer generates two files. The first one contains all graph nodes and the second file contains the ways.

#### Server
The Server jar will be generated in  `Server/target/osm-server.jar`. Run it via `java -jar osm-server.jar <nodefile> <wayfile>`. Like the importer working with large graphs requires a lot of memory. You can increase the performance by using the same extra arguments as described for the importer.  
After starting the server it needs some time loading the graph from the files to memory. A message will be shown after this step has finished. Only **after** this step was completed the server accepts requests from clients.  
The application can be used by opening [http://localhost:4567](http://localhost:4567) in any webbrowser.

#### Further execution
Most of the functionality of the project was tested during development by using junit tests. Any of these tests are fully functional. Notice that they usually require some statically references test pbf-files stored in the modules test-resources folder. 
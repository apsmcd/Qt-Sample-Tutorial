# Qt-Sample-Tutorial
Tutorial for combining two Qt code samples on ESRI AppStudio: Analyze Viewshed and Display Device Location


### 1. Create two new apps in AppStudio

- Click on 'New App' in AppStudio
- Navigate to 'Samples' tab
- Double click on 'Analyze Viewshed' (one of the first samples at the time of writing), then wait for the app to be created
- Again: 'New App', 'Samples', and now scroll down and double click on Display Device Location
- Right click on each new app and click on 'Edit in Qt Creator'

![step0](https://user-images.githubusercontent.com/35122203/36710356-412a38e8-1b32-11e8-922e-4d1cfd73e4ca.PNG)

- When both apps are open in the Qt Creator, move on to step 2. Make sure you keep track of which code is which! You can either just remember or click on File->New File or Project->Qt->QML File (Qt Quick 2)->Choose->name it something that helps you remember which code is which->leave everything else as default->Finish!


### 2. Edit the code!

For reference: Analyze Viewshed = AVS; Display Device Location = DDL

- Select and copy DDL lines 22-23, then paste into lines 23-24 in AVS
  >   import QtPositioning 5.3
  
  >   import QtSensors 5.3

![step1](https://user-images.githubusercontent.com/35122203/36710661-412a9386-1b34-11e8-8d62-ac7ad9d9db53.PNG)

- Select and copy DDL lines 41-48, then paste into lines 43-50 in AVS 
>     property string compassMode: "Compass"
>     property string navigationMode: "Navigation"  
>     property string recenterMode: "Re-Center"
>     property string onMode: "On"
>     property string stopMode: "Stop"
>     property string closeMode: "Close"
>     property string currentModeText: stopMode
>     property string currentModeImage:"assets/Stop.png"
    
![step2](https://user-images.githubusercontent.com/35122203/36710711-966c22ce-1b34-11e8-8338-5eacb1712b8f.PNG)
    
- Select and copy DDL lines 74-85, then paste into lines 79-90 in AVS
>     // start the location display  
>     onLoadStatusChanged: {
>         if (loadStatus === Enums.LoadStatusLoaded) {
>             // populate list model with modes
>             autoPanListModel.append({name: compassMode, image:"assets/Compass.png"});
>             autoPanListModel.append({name: navigationMode, image:"assets/Navigation.png"});
>             autoPanListModel.append({name: recenterMode, image:"assets/Re-Center.png"});
>             autoPanListModel.append({name: onMode, image:"assets/Stop.png"});
>             autoPanListModel.append({name: stopMode, image:"assets/Stop.png"});
>             autoPanListModel.append({name: closeMode, image:"assets/Close.png"});
>         }
>     }

![step3](https://user-images.githubusercontent.com/35122203/36711115-827afa22-1b36-11e8-9db9-957af1deb626.PNG)

- Select and copy DDL lines 88-93, then paste into lines 102-107 in AVS
>     // set the location display's position source
>      locationDisplay {
>          positionSource: PositionSource {
>          }
>          compass: Compass {}
>      }

![step3_2](https://user-images.githubusercontent.com/35122203/36711147-be7acb1a-1b36-11e8-83ad-bf2603bd9911.PNG)

- Select and copy DDL lines 104-220, then paste into lines 269-385 in AVS
>     ListView {
>         id: autoPanListView
>         anchors {
>             right: parent.right
>             bottom: parent.bottom
>             margins: 10 * scaleFactor
>         }
>         visible: false
>         width: parent.width
>         height: 300 * scaleFactor
>         spacing: 10 * scaleFactor
>         model: ListModel {
>             id: autoPanListModel
>         }
>          delegate: Row {
>             id: autopanRow
>             anchors.right: parent.right
>             spacing: 10
>              Text {
>                 text: name
>                 font.pixelSize: 25 * scaleFactor
>                 color: "white"
>                 MouseArea {
>                     anchors.fill: parent
>                     // When an item in the list view is clicked
>                     onClicked: {
>                         autopanRow.updateAutoPanMode();
>                     }
>                 }
>             }
>              Image {
>                 source: image
>                 width: 40 * scaleFactor
>                 height: width
>                 MouseArea {
>                     anchors.fill: parent
>                     // When an item in the list view is clicked
>                     onClicked: {
>                         autopanRow.updateAutoPanMode();
>                     }
>                 }
>             }
>              // set the appropriate auto pan mode
>             function updateAutoPanMode() {
>                 switch (name) {
>                 case compassMode:
>                     mapView.locationDisplay.autoPanMode = Enums.LocationDisplayAutoPanModeCompassNavigation;
>                     mapView.locationDisplay.start();
>                     break;
>                 case navigationMode:
>                     mapView.locationDisplay.autoPanMode = Enums.LocationDisplayAutoPanModeNavigation;
>                     mapView.locationDisplay.start();
>                     break;
>                 case recenterMode:
>                     mapView.locationDisplay.autoPanMode = Enums.LocationDisplayAutoPanModeRecenter;
>                     mapView.locationDisplay.start();
>                     break;
>                 case onMode:
>                     mapView.locationDisplay.autoPanMode = Enums.LocationDisplayAutoPanModeOff;
>                     mapView.locationDisplay.start();
>                     break;
>                 case stopMode:
>                     mapView.locationDisplay.stop();
>                     break;
>                 }
>                  if (name !== closeMode) {
>                     currentModeText = name;
>                     currentModeImage = image;
>                 }
>                  // hide the list view
>                 currentAction.visible = true;
>                 autoPanListView.visible = false;
>             }
>         }
>     }
>      Row {
>         id: currentAction
>         anchors {
>             right: parent.right
>             bottom: parent.bottom
>             margins: 25 * scaleFactor
>         }
>         spacing: 10
>          Text {
>             text: currentModeText
>             font.pixelSize: 25 * scaleFactor
>             color: "white"
>             MouseArea {
>                 anchors.fill: parent
>                 onClicked: {
>                     currentAction.visible = false;
>                     autoPanListView.visible = true;
>                 }
>             }
>         }
>          Image {
>             source: currentModeImage
>             width: 40 * scaleFactor
>             height: width
>             MouseArea {
>                 anchors.fill: parent
>                 onClicked: {
>                     currentAction.visible = false;
>                     autoPanListView.visible = true;
>                 }
>             }
>         }
>     }

![step4](https://user-images.githubusercontent.com/35122203/36711566-e2ba1ca4-1b38-11e8-87ab-ddd6e1b6ce57.PNG)

![step4_2](https://user-images.githubusercontent.com/35122203/36711575-eab08ad8-1b38-11e8-912d-d734888ba92f.PNG)


### 3. Change text color
- In the AVS code, in the Text blocks on lines 292 and 363, change 'color' to "black"

![step5](https://user-images.githubusercontent.com/35122203/36711672-79017022-1b39-11e8-8690-1428b3c9e65a.PNG)

![step5_2](https://user-images.githubusercontent.com/35122203/36711681-81ba22c2-1b39-11e8-9965-0a7a8195f64a.PNG)


### 4. Move over assets images
- For both AVS and DDL codes: in the sidebar under 'Projects', click the carrot next to the MyApp folder
- Right click on the 'assets' folder, then click 'Show in Explorer'

![step6](https://user-images.githubusercontent.com/35122203/36711697-9d51f348-1b39-11e8-9478-61f68c77d70d.PNG)

- With file explorers open for both 'assets' folders, select Close, Compass, Navigation, On, Re-Center, and Stop from the DDL assets folder, copy these, and paste them into the AVS assets folder

![step6_2](https://user-images.githubusercontent.com/35122203/36711706-a788e808-1b39-11e8-9792-05bbea8e4190.PNG)

### 5. Change map center and zoom
- Adjust lines 94, 95, and 98 (x, y, and targetScale) to suit your desired initial map center and extent

>     ViewpointCenter {
>         Point {
>             x: -120.7401
>             y: 47.7511
>             spatialReference: SpatialReference.createWgs84()
>         }
>         targetScale: 10500000
>     }

#### Note:
  > You may also change the basemap on line 77--I left it as topographic because it suits the viewshed analysis

        Map {
            BasemapTopographic {}


### 6. The full code!
> The combined Analyze Viewshed and Display Device Location code:

     /* Copyright 2017 Esri
     *
     * Licensed under the Apache License, Version 2.0 (the "License");
     * you may not use this file except in compliance with the License.
     * You may obtain a copy of the License at
     *
     *    http://www.apache.org/licenses/LICENSE-2.0
     *
     * Unless required by applicable law or agreed to in writing, software
     * distributed under the License is distributed on an "AS IS" BASIS,
     * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
     * See the License for the specific language governing permissions and
     * limitations under the License.
     *
     */

    import QtQuick 2.7
    import QtQuick.Layouts 1.1
    import QtQuick.Controls 2.1
    import QtQuick.Controls.Material 2.1
    import QtGraphicalEffects 1.0
    import QtQuick.Dialogs 1.2
    import QtPositioning 5.3
    import QtSensors 5.3

    import ArcGIS.AppFramework 1.0
    import ArcGIS.AppFramework.Controls 1.0
    import Esri.ArcGISRuntime 100.1

    import "controls" as Controls

    App {
        id: app
        width: 414
        height: 736
        function units(value) {
            return AppFramework.displayScaleFactor * value
        }
        property real scaleFactor: AppFramework.displayScaleFactor
        property int baseFontSize : app.info.propertyValue("baseFontSize", 15 * scaleFactor) + (isSmallScreen ? 0 : 3)
        property bool isSmallScreen: (width || height) < units(400)

        property string compassMode: "Compass"
        property string navigationMode: "Navigation"
        property string recenterMode: "Re-Center"
        property string onMode: "On"
        property string stopMode: "Stop"
        property string closeMode: "Close"
        property string currentModeText: stopMode
        property string currentModeImage:"assets/Stop.png"

        property bool viewshedInProgress: false
        property GeoprocessingJob viewshedJob: null
        property string statusText: ""

        Page{
            anchors.fill: parent
            header: ToolBar{
                id:header
                width: parent.width
                height: 50 * scaleFactor
                Material.background: "#8f499c"
                Controls.HeaderBar{}
            }

            // sample starts here ------------------------------------------------------------------
            contentItem: Rectangle{
                anchors.top:header.bottom

                // Map view UI presentation at top
                MapView {
                    id: mapView
                    anchors.fill: parent

                    // Create map with topographic basemap and initial viewpoint
                    Map {
                        BasemapTopographic {}

                        // start the location display
                        onLoadStatusChanged: {
                            if (loadStatus === Enums.LoadStatusLoaded) {
                                // populate list model with modes
                                autoPanListModel.append({name: compassMode, image:"assets/Compass.png"});
                                autoPanListModel.append({name: navigationMode, image:"assets/Navigation.png"});
                                autoPanListModel.append({name: recenterMode, image:"assets/Re-Center.png"});
                                autoPanListModel.append({name: onMode, image:"assets/Stop.png"});
                                autoPanListModel.append({name: stopMode, image:"assets/Stop.png"});
                                autoPanListModel.append({name: closeMode, image:"assets/Close.png"});
                            }
                        }

                        ViewpointCenter {
                            Point {
                                x: -120.7401
                                y: 47.7511
                                spatialReference: SpatialReference.createWgs84()
                            }
                            targetScale: 10500000
                        }
                    }

                    // set the location display's position source
                    locationDisplay {
                        positionSource: PositionSource {
                        }
                        compass: Compass {}
                    }

                    // Create the graphics overlays for the input and output
                    GraphicsOverlay {
                        id: inputOverlay

                        // Set the size and color properties for the simple renderer
                        SimpleRenderer {
                            SimpleMarkerSymbol {
                                color: "red"
                                style: Enums.SimpleMarkerSymbolStyleCircle
                                size: 12
                            }
                        }
                    }

                    GraphicsOverlay {
                        id: resultsOverlay

                        // Set the size and color properties for the simple renderer
                        SimpleRenderer {
                            SimpleFillSymbol {
                                color: Qt.rgba(0.88, 0.46, 0.16, 0.4)
                            }
                        }
                    }

                    // Set up signal handler for the mouse clicked signal
                    onMouseClicked: {
                        // The geoprocessing task is still executing, don't do anything else (i.e. respond to
                        // more user taps) until the processing is complete.
                        if (viewshedInProgress)
                            return;

                        // Indicate that the geoprocessing is running
                        viewshedInProgress = true;

                        // Clear previous user click location and the viewshed geoprocessing task results
                        inputOverlay.graphics.clear();
                        resultsOverlay.graphics.clear();

                        // Create a marker graphic where the user clicked on the map and add it to the existing graphics overlay
                        var inputGraphic = ArcGISRuntimeEnvironment.createObject("Graphic", {geometry: mouse.mapPoint});
                        inputOverlay.graphics.append(inputGraphic);

                        // Execute the geoprocessing task
                        viewshedTask.calculateViewshed(mouse.mapPoint);
                    }
                }

                // Create the GeoprocessingTask
                GeoprocessingTask {
                    id: viewshedTask
                    url: "https://sampleserver6.arcgisonline.com/arcgis/rest/services/Elevation/ESRI_Elevation_World/GPServer/Viewshed"

                    onErrorChanged: {
                        viewshedInProgress = false;
                        if (error)
                            showErrorDialog(error);
                    }

                    // function to caclulate the viewshed
                    function calculateViewshed(location) {
                        // Create a new feature collection table based upon point geometries using the current map view spatial reference
                        var inputFeatures = ArcGISRuntimeEnvironment.createObject("FeatureCollectionTable", {
                                                                                      geometryType: Enums.GeometryTypePoint,
                                                                                      spatialReference: SpatialReference.createWebMercator()
                                                                                  });

                        // Create a new feature from the feature collection table. It will not have a coordinate location (x,y) yet
                        var inputFeature = inputFeatures.createFeature();

                        // Assign a physical location to the new point feature based upon where the user clicked on the map view
                        inputFeature.geometry = location;

                        // connect to addFeature status changed signal
                        inputFeatures.addFeatureStatusChanged.connect(function() {
                            if (inputFeatures.addFeatureStatus === Enums.TaskStatusCompleted) {
                                // Create the parameters that are passed to the used geoprocessing task
                                var viewshedParameters = ArcGISRuntimeEnvironment.createObject("GeoprocessingParameters", {
                                                                                                   executionType: Enums.GeoprocessingExecutionTypeSynchronousExecute
                                                                                               });

                                // Request the output features to use the same SpatialReference as the map view
                                viewshedParameters.outputSpatialReference = SpatialReference.createWebMercator();

                                // Add an input location to the geoprocessing parameters
                                var inputs = {};
                                inputs["Input_Observation_Point"] = ArcGISRuntimeEnvironment.createObject("GeoprocessingFeatures", { features: inputFeatures });
                                viewshedParameters.inputs = inputs;

                                // Create the job that handles the communication between the application and the geoprocessing task
                                viewshedJob = viewshedTask.createJob(viewshedParameters);

                                // Create signal handler for the job
                                viewshedJob.jobStatusChanged.connect(viewshedJobHandler);

                                // start the job
                                viewshedJob.start();
                            }
                        });

                        // Add the new feature with (x,y) location to the feature collection table
                        inputFeatures.addFeature(inputFeature);
                    }

                    // function to handle the job as it changes its status
                    function viewshedJobHandler() {
                        if (viewshedJob.jobStatus === Enums.JobStatusFailed) {
                            showErrorDialog(viewshedJob.error);
                            viewshedInProgress = false;
                            statusText = "Job failed.";
                        } else if (viewshedJob.jobStatus === Enums.JobStatusStarted) {
                            viewshedInProgress = true;
                            statusText = "Job in progress...";
                        } else if (viewshedJob.jobStatus === Enums.JobStatusPaused) {
                            viewshedInProgress = false;
                            statusText = "Job paused...";
                        } else if (viewshedJob.jobStatus === Enums.JobStatusSucceeded) {
                            // handle the results
                            processResults(viewshedJob.result);
                            viewshedInProgress = false;
                            statusText = "Job succeeded.";
                        }
                    }

                    // function to handle the results from the GeoprocessingJob
                    function processResults(result) {
                        // Get the results from the outputs as GeoprocessingFeatures
                        var viewshedResultFeatures = result.outputs["Viewshed_Result"];

                        // Add all the features from the result feature set as a graphics to the map
                        var viewshedAreas = viewshedResultFeatures.features.iterator;
                        while (viewshedAreas.hasNext) {
                            var feat = viewshedAreas.next();
                            var graphic = ArcGISRuntimeEnvironment.createObject("Graphic", {
                                                                                    geometry: feat.geometry
                                                                                });
                            resultsOverlay.graphics.append(graphic);
                        }
                    }

                    function showErrorDialog(error) {
                        messageDialog.title = "Error";
                        messageDialog.text = "Executing geoprocessing failed.";
                        messageDialog.detailedText = error ? error.message : "Unknown error";
                        messageDialog.open();
                    }
                }

                // Create rectangle to display the status
                Rectangle {
                    anchors {
                        margins: -10 * scaleFactor
                        fill: statusColumn
                    }
                    color: "lightgrey"
                    radius: 5
                    border.color: "black"
                    opacity: 0.85
                }

                ListView {
                    id: autoPanListView
                    anchors {
                        right: parent.right
                        bottom: parent.bottom
                        margins: 10 * scaleFactor
                    }
                    visible: false
                    width: parent.width
                    height: 300 * scaleFactor
                    spacing: 10 * scaleFactor
                    model: ListModel {
                        id: autoPanListModel
                    }

                    delegate: Row {
                        id: autopanRow
                        anchors.right: parent.right
                        spacing: 10

                        Text {
                            text: name
                            font.pixelSize: 25 * scaleFactor
                            color: "black"
                            MouseArea {
                                anchors.fill: parent
                                // When an item in the list view is clicked
                                onClicked: {
                                    autopanRow.updateAutoPanMode();
                                }
                            }
                        }

                        Image {
                            source: image
                            width: 40 * scaleFactor
                            height: width
                            MouseArea {
                                anchors.fill: parent
                                // When an item in the list view is clicked
                                onClicked: {
                                    autopanRow.updateAutoPanMode();
                                }
                            }
                        }

                        // set the appropriate auto pan mode
                        function updateAutoPanMode() {
                            switch (name) {
                            case compassMode:
                                mapView.locationDisplay.autoPanMode = Enums.LocationDisplayAutoPanModeCompassNavigation;
                                mapView.locationDisplay.start();
                                break;
                            case navigationMode:
                                mapView.locationDisplay.autoPanMode = Enums.LocationDisplayAutoPanModeNavigation;
                                mapView.locationDisplay.start();
                                break;
                            case recenterMode:
                                mapView.locationDisplay.autoPanMode = Enums.LocationDisplayAutoPanModeRecenter;
                                mapView.locationDisplay.start();
                                break;
                            case onMode:
                                mapView.locationDisplay.autoPanMode = Enums.LocationDisplayAutoPanModeOff;
                                mapView.locationDisplay.start();
                                break;
                            case stopMode:
                                mapView.locationDisplay.stop();
                                break;
                            }

                            if (name !== closeMode) {
                                currentModeText = name;
                                currentModeImage = image;
                            }

                            // hide the list view
                            currentAction.visible = true;
                            autoPanListView.visible = false;
                        }
                    }
                }

                Row {
                    id: currentAction
                    anchors {
                        right: parent.right
                        bottom: parent.bottom
                        margins: 25 * scaleFactor
                    }
                    spacing: 10

                    Text {
                        text: currentModeText
                        font.pixelSize: 25 * scaleFactor
                        color: "black"
                        MouseArea {
                            anchors.fill: parent
                            onClicked: {
                                currentAction.visible = false;
                                autoPanListView.visible = true;
                            }
                        }
                    }

                    Image {
                        source: currentModeImage
                        width: 40 * scaleFactor
                        height: width
                        MouseArea {
                            anchors.fill: parent
                            onClicked: {
                                currentAction.visible = false;
                                autoPanListView.visible = true;
                            }
                        }
                    }
                }

                Column {
                    id: statusColumn
                    anchors {
                        right: parent.right
                        top: parent.top
                        margins: 20 * scaleFactor
                    }

                    Text {
                        anchors.margins: 5 * scaleFactor
                        visible: !viewshedInProgress
                        text: "Click map to execute viewshed analysis"
                        font.pixelSize: 12 * scaleFactor
                    }

                    Row {
                        anchors.margins: 5 * scaleFactor
                        visible: viewshedInProgress
                        spacing: 10 * scaleFactor

                        BusyIndicator {
                            Material.accent:"#8f499c"
                            anchors.verticalCenter: parent.verticalCenter
                            width: 30 * scaleFactor
                            height: width
                        }

                        Text {
                            anchors.verticalCenter: parent.verticalCenter
                            text: statusText
                            font.pixelSize: 12 * scaleFactor
                        }
                    }
                }

                // Dialog to display errors
                MessageDialog {
                    id: messageDialog
                    title: "Error"
                    text: "Executing geoprocessing failed."
                }

            }
        }

        // sample ends here ------------------------------------------------------------------------
        Controls.DescriptionPage{
            id:descPage
            visible: false
        }
     }

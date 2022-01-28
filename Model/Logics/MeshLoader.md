Opis logiki
===========

Logika pozwala na wczytanie drzewa meshy z teksturami i materiałami.

Tworzenie
---------

     {
        "Event" : "NodeLogicEvent",
        "Command" : "AddNodeLogic",
        "EventID" : "1",
        "NodeName" : "rootNode",
        "SceneName" : "sceneFromEnv@ MESH_TEST_SCENE",
        "Action" : 
        {
            "logic" : 
            {
                "type" : "mesh_loader",
                "timelinePath" : "sceneFromEnv@ MESH_TEST_SCENE%default",
                "assetPath": "meshes/StarWarsFighter/ARC.FBX"
            }
        }
     }

Ładowanie mesha
---------------

     {
        "Event" : "NodeLogicEvent",
        "Command" : "LogicAction",
        "EventID" : "1",
        "NodeName" : "rootNode",
        "SceneName" : "sceneFromEnv@ MESH_TEST_SCENE",
        "Action" :
        {
                      "Action":"Load",
                      "textureEnabled":"true",
                      "materialEnabled":"true"
            }
     }

GetAssetPath / SetAssetPath
---------------------------

     {
        "Event" : "NodeLogicEvent",
        "Command" : "LogicAction",
        "SceneName" : "sceneFromEnv@ MESH_TEST_SCENE",
        "NodeName" : "rootNode",
        "Action" :
        {
            "Action" : "GetAssetPath"
        }
     }

     {
        "Event" : "NodeLogicEvent",
        "Command" : "LogicAction",
        "SceneName" : "sceneFromEnv@ MESH_TEST_SCENE",
        "NodeName" : "rootNode",
        "Action" :
        {
                     "Action" : "SetAssetPath",
                     "assetPath" : "meshes/StarWarsFighter/ARC.FBX"
        }
     }

Mesh Info
---------

     {
        "Event" : "NodeLogicEvent",
        "Command" : "LogicAction",
        "SceneName" : "sceneFromEnv@ MESH_TEST_SCENE",
        "NodeName" : "rootNode",
        "Action" :
        {
            "Action" : "MeshInfo"
        }
     }

Odpowiedź:

     {
       "EventID" : "1",
       "Success" : "true",
       "cmd" : "LogicAction",
       "meshNum" : "2",
       "meshes" : [
          {
             "material" : "true",
             "name" : "ARC_170_LEE_RAY:polySurface1394_Material0"
          },
          {
             "material" : "true",
             "name" : "ARC_170_LEE_RAY:polySurface1394_Material1",
             "texture" : "C:\\Users\\admin\\Dropbox\\bv_media\\meshes\\StarWarsFighter\\ARC170_TXT_VERSION_4_D.jpg"
          }
       ]
     }
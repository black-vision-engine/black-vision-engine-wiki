# Plugin RoundedRect

**Name**: rounded rect

**UID**: DEFAULT_ROUNDEDRECT


Generates rectangle with rounded corners. Rectangle can be solid or outlined.


##Parameters


Parameter name         	| Type      	| Default Value    	| Description
----------------------- | -------------	| -----------------	| -----------
size		         	| vec2	   		| 1.0, 1.0			| Width and height of rectangle.
bevels					| vec4			| 0.1, 0.1, 0.1, 0.1| Radius of corners circles.
tesselation				| int			| 10				| Tesselation of corners.
stretch					| float			| 0.0f				| Stretch rectangle in x axis.
use outline				| bool			| false				| If true, plugin will generate outlined rectangle. Otherwise rect will be filled inside.
outline width			| float			| 0.0f				| Width of outline. This parameter is used only if **use outline** is set to true.
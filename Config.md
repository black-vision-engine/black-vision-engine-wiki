[TOC]

# Config

**Full example:**


```
#!xml
<config>

	<property name="PMFolder" value="../bv_media/" />
    
	<property name="UseReadbackAPI" value="true" />
	<property name="Resolution" value="HD" />
    
	<property name="Debug">
		<property name="LoadSceneFromEnv" value="false" />
        <property name="LoadSceneFromProjectManager" value="" />
		<property name="SceneFromEnvName" value="TWO_TEXTURED_RECTANGLES" />

        <property name="CommandsDebugLayer">
            <property name="UseDebugLayer" value="false" />
            <property name="FilePath" value="../../../../Logs/" />
        </property>		
	</property>
    
    <property name="Plugins" >
        <property name="Textures" >
            <property name="OnFailedLoadBehavior" value="LoadChecker" />
        </property>
    </property>
	
    <property name="Audio" >
        <property name="GlobalGain" value="1.0" />
    </property>
    
	<property name="Renderer">
		<property name="MaxFPS" value="5000" />
		<property name="TimerFPS" value="50" />
        
        <property name="FrameBufferSize">
            <property name="Width" value="500" />
            <property name="Height" value="600" />
        </property>
        
        <property name="ClearColor" value="0,0,0,0" />
        
	</property>
	
    <property name="SharedMemory">
        <property name="Enable" value="true" />
        <property name="Name" value="BV" />
        <property name="Width" value="1920" />
        <property name="Height" value="1080" />
    </property>
	
	<property name="Application">
		<property name="VSync" value="false" />
		<property name="Window">
            <property name="FullScreen" value="false" />
			<property name="Mode" value="Windowed" />        <!-- rodzaj wyswietlanego okienka MultipleScreens|Windowed -->
			<property name="Size">
				<property name="Width" value="1920" />
				<property name="Height" value="1080" />
			</property>
		</property>
        
        <property name="Events" >
            <property name="MaxLoopUpdateTime" value="20" />
            <property name="EnableLockingQueue" value="false" />
        </property>
	</property>
	
	<property name="Network">
		<property name="SocketServer">
			<property name="Port" value="11011" />
		</property>
	</property>
    
	<property name="Camera">
        <property name="IsPerspective" value="true" />
		<property name="FOV" value="60" />
		<property name="Position" value="0,0,5" />
		<property name="Direction" value="0,0,-1" />
	</property>

    <videocards>
        <videocard name="BlueFish" deviceID="1" referenceMode="FreeRun" referenceH="0" referenceV="0" >
            <channels>
                <channel name="A" >
                    <output linkedVideoOutput="0" type="FILL_KEY" resolution="1080" refresh="5000" interlaced="true" flipped="true" />
				</channel>
                <channel name="B" >
                    <output linkedVideoOutput="1" type="FILL_KEY" resolution="1080" refresh="5000" interlaced="true" flipped="true" />
				</channel>
				<channel name="A" >
                    <input linkedVideoInput="0" type="FILL" resolution="1080" />
				</channel>
            </channels>
        </videocard>
    </videocards>

	<RenderChannels>
	
        <RenderChannel id="0" enabled="true" >
		</RenderChannel>
		
        <RenderChannel id="1" enabled="false" />
        <RenderChannel id="2" enabled="false" />
        <RenderChannel id="3" enabled="false" />
    </RenderChannels>
	
</config>

```

## Application

Key                                     | Values        | Default Value        | Description
--------------------------------------- | ------------- | -------------------- | -----------
Application/VSync                       | bool          | true                 | Enable/Disable vertical synchronization with screen.
Application/Window/FullScreen           | bool          | false                | Set fullscreen mode.
Application/Window/Mode                 | enum          | WINDOWED             | MultipleScreens, WINDOWED. Ignored if FullScreen flag is st to true.
Application/Window/Size/Width           | enum          | Int32                | Width of application window.
Application/Window/Size/Height          | enum          | Int32                | Height of application window.


### Events

Key                                     | Values        | Default Value        | Description
--------------------------------------- | ------------- | -------------------- | -----------
Application/Events/MaxLoopUpdateTime    | UInt32        | 20                   | Maximal time spent on processing events during single frame.
Application/Events/EnableLockingQueue   | bool          | false                | Pozawala wysyłanie eventu do blokowania kolejki eventów na określona liczbę ramek. [Zobacz](Events/EngineStateEvent)

**Examples:**

```
<config>
	<property name="Application">

        <property name="Events" >
            <property name="MaxLoopUpdateTime" value="20" />
            <property name="EnableLockingQueue" value="false" />
        </property>
	</property>
</config>
```

## Renderer

Key                                     | Values        | Default Value        | Description
--------------------------------------- | ------------- | -------------------- | -----------
MaxFPS                                  | UInt32        | 60000                | 
TimerFPS                                | UInt32        | 60                   | 
ClearColor                              | vec4          | 0.0, 0.0, 0.0, 0.0   | Default color to render target clear operation.
FrameBufferSize/Width                   | UInt32        | 1920                 | 
FrameBufferSize/Height                  | UInt32        | 1080                 | 



```
#!xml
<config>
    <property name="Renderer">
        <property name="MaxFPS" value="5000" />
        <property name="TimerFPS" value="50" />
        
        <property name="FrameBufferSize">
            <property name="Width" value="500" />
            <property name="Height" value="600" />
        </property>
        
        <property name="ClearColor" value="0,0,0,0" />
        
    </property>
</config>
```


## SharedMemory

Key                                     | Values        | Default Value        | Description
--------------------------------------- | ------------- | -------------------- | -----------
Enable                                  | bool          | false                | Enables rendering to shared memory.
Width/Height                            | UInt32        | 1920x1080            | Size od shared memory buffer.
Name                                    | string        | "BV"                 | Name of shared memory buffer to access in external application.

**Examples:**

```
<config>
    <property name="SharedMemory">
        <property name="Enable" value="false" />
        <property name="Name" value="BV" />
        <property name="Width" value="1920" />
        <property name="Height" value="1080" />
    </property>
</config>
```

## Network

Key                                     | Values        | Default Value        | Description
--------------------------------------- | ------------- | -------------------- | -----------
SocketServer/Port                       | UInt32        | 12345                | Connection port for external tools (events API)

**Examples:**

```
<config>
	<property name="Network">
		<property name="SocketServer">
			<property name="Port" value="11011" />
		</property>
	</property>
</config>
```

## Plugins

### Textures

Key                                     | Values        | Default Value        | Description
--------------------------------------- | ------------- | -------------------- | -----------
Plugins/Textures/OnFailedLoadBehavior   | string        | LoadChecker          | Texture plugins behavior, when loaded texture doesn't exist. Posible values: **LoadChecker**, **LoadTransparent**, **LeavePrevious**.

**Example:**

```
<config>
    <property name="Plugins" >
        <property name="Textures" >
            <property name="OnFailedLoadBehavior" value="LoadChecker" />
        </property>
    </property>
</config>
```

## Audio

Key                                     | Values        | Default Value        | Description
--------------------------------------- | ------------- | -------------------- | -----------
GlobalGain                              | float         | 1.0                  | Audio global gain.

## Debug

Key                                     | Values        | Default Value        | Description
--------------------------------------- | ------------- | -------------------- | -----------
LoadSceneFromEnv                        | bool          | false                | BV will load scene using environment variable.
SceneFromEnvName                        | string        | ""                   | One of embeded test scenes.
LoadSceneFromProjectManager             | string        | ""                   | Scene name from ProjectManager to load.
CommandsDebugLayer/UseDebugLayer        | bool          | false                | Record all events coming to BV through TCP connection.
CommandsDebugLayer/FilePath             | string        | ""                   | File to save recorded events.
**Examples:**

```
<config>
	<property name="Debug">
		<property name="LoadSceneFromEnv" value="false" />
        <property name="LoadSceneFromProjectManager" value="" />
		<property name="SceneFromEnvName" value="TWO_TEXTURED_RECTANGLES" />

        <property name="CommandsDebugLayer">
            <property name="UseDebugLayer" value="false" />
            <property name="FilePath" value="../../../../Logs/" />
        </property>		
	</property>
</config>
```

## Camera

Default camera parameters. **Note that these values aren't used in implementation** and remain for future use.

```
<config>
	<property name="Camera">
        <property name="IsPerspective" value="true" />
		<property name="FOV" value="60" />
		<property name="Position" value="0,0,5" />
		<property name="Direction" value="0,0,-1" />
	</property>
</config>
```

## Render channels


```
#!xml
<config>
	<RenderChannels>
	
        <RenderChannel id="0" enabled="true" >
			<VideoOutput id="0" />
		</RenderChannel>
		
        <RenderChannel id="1" enabled="true" >
			<VideoOutput id="1" />
		</RenderChannel>
		
        <RenderChannel id="2" enabled="false" />
        <RenderChannel id="3" enabled="false" />
    </RenderChannels>
</config>
```



## Video cards


```
#!xml
<config>



    <videocards>
        <videocard name="BlueFish" deviceID="1" referenceMode="FreeRun" referenceH="0" referenceV="0" >
            <channels>
                <channel name="A" >
                    <output linkedVideoOutput="0" type="FILL_KEY" resolution="1080" refresh="5000" interlaced="true" flipped="true" />
				</channel>
                <channel name="B" >
                    <output linkedVideoOutput="1" type="FILL_KEY" resolution="1080" refresh="5000" interlaced="true" flipped="true" />
				</channel>
				<channel name="A" >
                    <input linkedVideoInput="0" type="FILL" resolution="1080" />
				</channel>
            </channels>
        </videocard>
    </videocards>

</config>
```
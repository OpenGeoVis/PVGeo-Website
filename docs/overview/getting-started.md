!!! warning
    PVGeo is compatible only with the nightly build of ParaView at this time. You must download the **nightly-build** version of ParaView [found here](https://www.paraview.org/download/)

!!! info
    If you have an idea for a macro, plugin, or would like to see how we would address a geoscientific visualization problem with ParaView, please post your thoughts on the [**issues page**](https://github.com/OpenGeoVis/PVGeo/issues) or get involved with the *PVGeo* community on Slack to discuss adding new features: <script async defer src="http://slack.pvgeo.org/slackin.js"></script>

## A Brief Introduction to ParaView

ParaView is an open-source platform that can visualize 2D, 3D, and 4D (time-varying) datasets. ParaView can process multiple very large data sets in parallel then later collect the results to yield a responsive graphics environment with which a user can interact. The better the processor and graphics hardware the machine or machines hosting the software, the faster and better ParaView will run. However, it can run quite well on a laptop with a standard graphics card such as a MacBook Pro.

Since ParaView is an open source application, anyone can download the program and its source code for modifications. The easiest way to get started with ParaView is to download the compiled binary installers for your operating system from [**here**](https://www.paraview.org/download/).

For further help, check out the [documentation](https://www.paraview.org/documentation/) provided by Kitware. In particular, the two worth looking through for a quick tour of ParaView are the **The ParaView Guide** and **The ParaView Tutorial.** One is a tutorial of the ParaView software and shows the user how to create sources, apply filters, and more. The other is a guide on how to do scripting, macros, and more intense use of the application.

### Install ParaView

!!! warning
    PVGeo is compatible only with the nightly build of ParaView at this time. You must download the **nightly-build** version of ParaView [found here](https://www.paraview.org/download/)

Open the downloaded installer from [**ParaView's website**](https://www.paraview.org/download/) and follow the prompts with the installer.

Tour around software:
Take a look at Section 2.1 of **The ParaView Tutorial** for details of the application’s GUI environment. Chapter 2 of the tutorial as a whole does an excellent job touring the software and its workflow for those unfamiliar with the software and its general capabilities.


??? tip "Tip: State Files"
    One convenient feature is to save the state of the ParaView environment. This saves all the options you selected for all the filters you applied to visualize some data. Select *File->Save State…* (*Note:* this saves the absolute path of the files loaded into ParaView, so be sure to select *Search for Files Under Directory...* when opening these state files).


----------


## Install *PVGeo*

We highly recomend using Anaconda to manage you python virtual environments and we know installation via Anaconda python distributions will work on Mac, Windows, and Linux operating systems. To begin using the *PVGeo* python package, create a new virtual environment and install *PVGeo* through pip.

```bash
$ conda create -n PVGeoEnv python=2.7
```

!!! warning "Use Python 2.7!!!"
    If you'd like to link PVGeo to ParaView, you must use a **Python 2.7** virtual environment.

```bash
$ source activate PVGeoEnv
(PVGeoEnv) $ pip install PVGeo
```

### Non-Windows Users

Now you must install VTK to your virtual environment. For Linux and Mac users, simply install VTK through `pip`:

```bash
# Now install VTK
(PVGeoEnv27) $ pip install vtk
```

Test the install (non-Windows):
```bash
(PVGeoEnv27) $ python -m PVGeo test
```


## Install PVGeo to ParaView

!!! warning
    PVGeo is compatible only with the nightly build of ParaView at this time. You must download the **nightly-build** version of ParaView [found here](https://www.paraview.org/download/)

To use the *PVGeo* library as plugins in ParaView, we must link the virtual environment that you installed *PVGeo* to ParaView's python environment and load a series of plugin files that wrap the *PVGeo* code base with ParaView's Graphical User Interface.


### Linking *PVGeo*
First, lets link *PVGeo*'s virtual environment to ParaView by setting up a `PYTHONPATH` and a `PV_PLUGIN_PATH` environmental variables. First, retrieve the needed paths from *PVGeo*. Do this by executing the following from your command line:

```bash
(PVGeoEnv) $ python -m PVGeo install
```

??? bug "Having Trouble?"
    Try executing the following command to debug the launcher creation (this will help us if you create an issue):

    ```bash
    (PVGeoEnv27) $ python -m PVGeo install echo
    ```

??? warning "You Should Upgrade ParaView's NumPy"
    Unfortunately, ParaView ships with a very old version of NumPy by default (v1.8.1) and PVGeo requires NumPy v1.13.x or later. Until ParaView upgrades its NumPy version, you must go into ParaView's application contents, delete NumPy, and create a new symbolic link to the NumPy installed in the `PVGeoEnv` virtual environment.

    If you do not do this, some algorithms will crash ParaView when used or some I/O operations might be a bit slow.

    **Steps:**

    1. Locate your installation of ParaView (`/Applications/ParaView 5.5.xxx/Contents` or `C:\Program Files\ParaView 5.5.xxxx\`)
    2. Navigate to the local Python site-packages:
        - Mac: `.../Contents/Python`
        - Windows: `...\bin\Lib\site-packages\`
    3. Delete the local version of NumPy (the `numpy` directory)
    4. Use the PYTHONPATH variable from the above code to find the NumPy installed along PVGeo.
    5. Create a shortcut or symbolic link for numpy within ParaView's python site-packages to the numpy folder in `PVGeoEnv`'s site-packages

#### Mac OS Users

That script will output the paths you need to set in the environmental variables moving forward. If you are on a Mac OS X computer then that script will output a shell command for you to execute for the install. If you are on a Mac, execute that command and skip to [Loading the Plugins](#loading-the-plugins)

#### Windows Users

Setting up environmental variables is a bit involved for Windows. Remember how we ran `:::bash python -m PVGeo install`? Well this created a new file on your Desktop called `PVGeoLauncher.bat`. We will use this file to safely launch ParaView it is own environment with environmental variable set properly.

1. Go to your Desktop and right-click to select **New->Shortcut**.

2. **Browse...** to the `PVGeoLauncher.bat` on your Desktop. Note sure where this file is? Check the output of the `install` command from above.

3. Click **Next** and give your shortcut a meaningful name like **ParaView+PVGeo** and select **Finish**.

4. Now right-click that newly created shortcut and select **Properties**.

5. For the **Start in** field, we will use the path to your ParaView installation (top-level). To discover this, go to where ParaView is installed. Likely in `C:\Program Files` and find the `ParaView 5.6-xxxxxx` folder. Go into that folder and then copy the full path by copying the path in the navigation bar at the top of the window. Paste this path into the **Start in** field.

6. Click **Apply** then **Okay**

7. Now launch ParaView using your new shortcut!

6. Test that the install worked: open the **Python Shell** and import the modules delivered in this repo by executing `import PVGeo` and `import pvmacros`. Errors should not arise but if they do, post to the [**issues page**](https://github.com/OpenGeoVis/PVGeo/issues) and the errors will be *immediately* addressed.


### Loading the Plugins

Now you must load the plugin files through ParaView's Plugin Manager. Select *Tools -> Manage Plugins* then select *Load New* on the bottom right of the popup dialog. Navigate to the directory declared in `PV_PLUGIN_PATH` and load each of the `.py` files. Once the files are loaded, expand them in the plugin manager and be sure to select *Auto Load*.

??? error "Not sure where your `PV_PLUGIN_PATH` is located?"

    Re-run the install command with an additional argument `echo`:

    ```bash
    (PVGeoEnv27) $ python -m PVGeo install echo
    ```

![Plugin Manager](plugin-manager.png) <!-- .element width="50%" -->

Now test that the install worked by ensuring the various categories for the PVGeo filters are in the **Filters** menu such as **PVGeo General Filters**. Errors should not arise but if they do, post to the [**issues page**](https://github.com/OpenGeoVis/PVGeo/issues) and the errors will be *immediately* addressed.


!!! help
    If an error arises or you are having trouble, feel free to join the *PVGeo* community on Slack and ask for help: <script async defer src="http://slack.pvgeo.org/slackin.js"></script>

    You can also post to the [**issues page**](https://github.com/OpenGeoVis/PVGeo/issues) if you think you are encountering a bug.



## Update *PVGeo*

```bash
(PVGeoEnv) $ pip install --upgrade PVGeo
```


--------------

## Using Outside Modules
If you installed *PVGeo* according to the instructions above, then any python package installed through pip in that virtual environment will be accessible in ParaView. For some further reading on using virtual environments with ParaView, see [this blog post](https://blog.kitware.com/using-pvpython-and-virtualenv/)
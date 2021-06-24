# CloneLens LensStudio version 4.0.1
Used M.H saliency model, resolved challenges on OPENGL Crash issues becuase 
we cannot user copyFrame() function for segmentation image for that. SOlution

https://youtu.be/69XAmkXiL6U after 30 minutes or so 

https://arbootcamp.com/snapchat-advanced/clone - check out this link to fix openGL crash problem 

In this method i have added delay to fix render target => mask texture replacement. Render target la render aaga munadiye ML ketathala 2 or 3 times tap apo tha munadi panna tap work aagi iruku, fixed that as well






**Time to fix a bug** (Steps from site, if above site down aagita, so atha copy paste [optional]

If you pushed your lens to your device (which you always do before submitting, right?), you may have noticed that it crashes when creating the clones. The UI works and the countdown works, but once you get to the first clone it crashes. This sort of problem is super frustrating to troubleshoot because everything was working fine inside Lens Studio and Snapchat doesn't tell us what went wrong.

So how do we troubleshoot this? Well we know when the issue is occurring. We know that the issue is either occurring with our createClone function or when we reset the delayed callback event. The first countdown works, but then either creating the clone or triggering the second one does not.

Let's start by taking a look at our createClone function. The first thing we are going to try is commenting out the line where we set the baseTex. Make the change, save the script, and push to your device. Didn't work? Comment out the next line where we set the opacityTex, save, and push to device. Hooray! Our lens doesn't crash anymore! But why? Unfortunately, I don't have an exact answer. I did ask around a bit and Ben Knutson shared that for whatever reason, the copyFrame function does not work with segmentation textures. But that is kind of a problem for us. How do we solve it?

Fortunately the answer is simple. We are going to pipe the Body Segmentation texture to a separate render target.

Create a new Render Target, name it "Mask" or something
Create a new camera, name it "Mask Cam" or something
Create a new layer and set the camera to just that layer
Set the camera type to orthographic
Set the camera's render target to the mask render target
Go to the Scene Config and drag the mask render target to the top of the list so it is rendered first
Add a screen image to the mask camera and make sure it is on the same layer as the camera
Change the stretch mode of the image to "Stretch" (I don't like to take any chances) and choose the Body Segmentation texture for the image texture
Select the clone controller script and select the mask render target for the mask texture input
Make sure any changes you made to the control script while troubleshooting are reverted (uncomment anything you commented out).
That's a long list, but all we are doing is setting up a new camera/render target pair on a new layer, outputting our segmentation texture to that, and then using that render target as our mask texture. Once that is done, your lens is now complete and will work on your device. Give it a try!


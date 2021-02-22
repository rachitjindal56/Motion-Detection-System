An interesting python project involving the use of a cv2 module for detecting the motion by using the difference in the frame technique.

A very simple but interesting Python packet for detecting the motion that can be deployed in CCTV cameras, autonomous cars for better stability in self-driving mode. 

The packet uses cv2 for detecting the frame difference and finding the contour.

import cv2

The module is very useful for image processing and provides a lot of handy functions to ease the work.

Capturing Frames:

cap = cv2.VideoCapture(0)

while True:

    # Reading the captured frames-> frame1, frame2
    ret,frame1 = cap.read()
    ret,frame2 = cap.read()
The above code block captures the frame in 'cap' and stores the frame in form of an array in the 'frame' variable. Here two frames, frame1 and frame2 taken to use the concept of difference in the values of two frames.

Image Preparation:

# Checking Difference in motion in 2 frames
    diff_frame = cv2.absdiff(frame1,frame2)

    # Converting BGR-> Gray and making frame blur
    gray = cv2.cvtColor(diff_frame,cv2.COLOR_BGR2GRAY)
    blur = cv2.GaussianBlur(gray,(5,5),0)
    _,thresh = cv2.threshold(blur,20,255,cv2.THRESH_BINARY)

    # Dilating the frame to make get uniform contour
    dilated = cv2.dilate(thresh,None,iterations=10)
In the above code packet, diff_frame stores the coordinates that are different in frame1 and frame2 to find irregularities in the two frames. The difference is then fed to convert it into grayscale to ensure uniformity while using contour.

The frame is blurred using GaussianBlur to cause saturation and dilated to make the process of finding contour area easy. All this acts as saturation on the frame to get more accurate motion results.

Motion Detection:

Here the returned frame after filtering undergoes the detection phase: 

    #  Finding the contours in the difference to get coordinates of motion 
    contour,x = cv2.findContours(dilated,cv2.RETR_TREE,cv2.CHAIN_APPROX_SIMPLE)
    
    for c in contour:

        # If the contourArea < 1000 ignore the frame
        if cv2.contourArea(c) < 1000:
            continue

        # Drawing rectangle around the moving object
        x,y,w,h = cv2.boundingRect(c)
        cv2.rectangle(frame1,(x,y),(x+w,y+h),(0,255,0),2)
        cv2.imshow("The Frame",frame1)


    # Press 'q' to exit the frame
    if cv2.waitKey(1) & 0xff == ord('q'):
        break

cap.release()
cv2.destroyAllWindows()
The contour stores coordinate that were different in the two frames in form of an array. Now, as we got the contour coordinates, but it will show all the motions, to saturate that we will use cv2.contourArea() to check for the area of the motion. Say, if cv2.contourArea(c) < 1000 ignore the contour or else mark the contour.
After this, get to coordinated to draw a rectangle around the moving object/part. The packet will be terminated by pressing 'q' which is stated by 0xff == ord('q'). You can even insert any other key for termination.

This project can be used for motion-sensing devices or IoT devices. 

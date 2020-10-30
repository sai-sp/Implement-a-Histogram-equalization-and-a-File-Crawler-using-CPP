# Implement-a-Histogram-equalization-and-a-File-Crawler-using-C++

1) Implement a Histogram equalization from scratch using C++
       Inroduction:
                    A histogram of a digital image represents intensity distribution by plotting bar graph with "X-axis as pixel intensity value and Y-axis as the frequency of its occurrence". Histogram Equalisation is a technique to adjust contrast levels and expand the intensity range in a digital image. Thus, it enhances the image which makes information extraction and further image processing easier.
        
      Theory:
Consider an image whose pixel values are confined to some specific range of values only. For eg, brighter image will have all pixels confined to high values. But a good image will have pixels from all regions of the image. So you need to stretch this histogram to either ends and that is what Histogram Equalization does. This normally improves the contrast of the image.

Code and Explanation:
    Step 1:
            // Read the image file
            Mat image = imread("file-name");
            // Check for failure
            if (image.empty())
                {
                  cout << "Could not open or find the image" << endl;
                  cin.get(); //wait for any key press
                  return -1;
                }
                This Code is used to implement a image from specified file.
                
   Step 2:
           cvtColor(image, image, COLOR_BGR2GRAY); 
            
   This code is used to convert image into "BGR" ( Here RGB and BGR is almost same, but has a little difference in it, where in RGB - Blue occupies a least                         significant area, where BGR - Red Oocupies a least significant area)
                
   Step 3:
          Mat hist_equalized_image;
          equalizeHist(image, hist_equalized_image); 
                   
   Here this line filters the normal image into Eqaulized image.
                   
   Step 4:
           //Define names of windows
           String windowNameOfOriginalImage = "Original Image"; 
           String windowNameOfHistogramEqualized = "Histogram Equalized Image";
           
           // Create windows with the above names
           
           namedWindow(windowNameOfOriginalImage, WINDOW_NORMAL);
           namedWindow(windowNameOfHistogramEqualized, WINDOW_NORMAL)
           // Show images inside the created windows.
          
           imshow(windowNameOfOriginalImage, image);
           imshow(windowNameOfHistogramEqualized, hist_equalized_image);
                   
   The above code segment will create windows and show images in them. For creating Histogram Eqaulized Image.
                   
   Step 5:
            waitKey(0); // Wait for any keystroke in the window
            destroyAllWindows(); //destroy all open window
            return 0;
               
   The program will wait until any key is pressed. After a key is pressed, all created windows will be destroyed and the program will exit.
   
   Finally we need implement the basic header file need to work and process images.
   




# Add images in Eagle

1. Create an image and export as **png**

2. Go to [online-convert.com](https://image.online-convert.com/convert-to-bmp), select **Monochrome** in *Apply filters* section and export as **bmp**

3. Now you have a black and white bitmap in windows format (bottom to top)

4. Create a new libray in Eagle, then a new Footprint

5. Run the *ULP* called **importBMP.ulp** 

6. Browse the .bmp and chose the **white** color

7. Set **scaled**, then **millimiters** and chose the size in mm of your image into your pcb

8. You have an image on layer 200, delete the bottom-left text (file path)

9. Select everything and chose the layer (e.g. tStop)

10. Save and exit from library creator

11. In your PCB add component and chose the one containing your image

12. You have your image!!!
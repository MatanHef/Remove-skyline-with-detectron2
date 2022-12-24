# remove-skyline-with-detectron2
Remove skyline image  by using a model to detect the skyline automatically.
Install and import detctron2. 
Import pytorch, numpy, os, json, cv2 and detectron2 utilities

The function remove_skyline get an image and return the image without the skyline and with annotation.  
Detectron2 config and a detectron2 DefaultPredictor to run Inference with a panoptic segmentation model, this model not detect just things but try to connect every pixel to some object (including backround and the category we want to detect - sky)
The predictor makes the entire image annotated, to use outputs we get the key "panoptic_seg".
this element is a touple contains matrix, every element of the matrix represent the annotated pixel by id and a dictionary that connect between the id and the category detected.

This for loop get the id number of the sky, if threr is no sky at the picture we get None
  id_sky=None
  for i in outputs["panoptic_seg"][1]:
    if i["isthing"]==0 and i["category_id"]==40:
        id_sky=i["id"]
        
 this for loop find the lowest row with sky:
  first_row=None 
  Id_matrix = np.asarray(outputs["panoptic_seg"][0].to("cpu"))
  for i in range (len(Id_matrix)):
    if id_sky in Id_matrix[i]:
      first_row=i
 
 Take out the skyline by slice the image from the lowest row that has been found and bottom of the picture.

# signpost (misc)
>Tracking down pictures can be hard. Surely you can't find where this giant ballpark is...

>Submit your flag as ictf{latitude,longitude}, with the coordinates of where the picture was taken rounded to three decimal places. Example: ictf{-42.443,101.333}
![signpost](images/signpost.png)

# Solution
Using Google reverse image search we find the full photo and its location 
https://www.alamy.com/stock-photo-directional-signs-in-san-francisco-indicating-distance-and-pointing-41412463.html
>San Francisco

One more hint to narrow down the search is "Gilroy Garlic Fries at Centerfield" at 50ft as shown in the full photo. With just another Google search we get a more specific location at Giants Promenade near Oracle Park. The rounded coordinates at this location to three decimal places are `37.778, -122.388`

# Flag
ictf{37.778,-122.388}
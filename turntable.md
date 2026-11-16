# Unity Turntable Camera

1. Move **Main Camera** to one side of your object and slightly above it. The camera’s starting position sets the orbit radius and height.
2. In Unity 6, choose **Assets > Create > Scripting > MonoBehaviour Script**. Name it `TurntableCamera`. Open it, replace its contents with the code below, and save.

   ```csharp
   using UnityEngine;

   public class TurntableCamera : MonoBehaviour
   {
       public Transform target;
       public float rotationSpeed = 30f; // Degrees per second

       private void LateUpdate()
       {
           if (target == null)
           {
               return;
           }

           Vector3 center = target.position;
           float angle = rotationSpeed * Time.deltaTime;

           transform.RotateAround(center, Vector3.up, angle);
           transform.LookAt(center);
       }
   }
   ```

3. Drag `TurntableCamera` from the **Project** window onto **Main Camera** in the **Hierarchy**.
4. Select **Main Camera**. Drag your object from the **Hierarchy** into the script’s **Target** field in the **Inspector**. If the object’s pivot is off-center, create an empty GameObject at the center you want and assign that as the target.
5. Press **Play** and adjust **Rotation Speed** in the **Inspector**. A negative value reverses direction, and `0` stops the orbit. Set your preferred speed outside Play Mode to keep it.
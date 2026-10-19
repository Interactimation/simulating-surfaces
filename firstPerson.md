# First-person Player 

## Capsule Setup

1. **Create the player**
   - GameObject → 3D Object → Capsule.
   - Rename it `Player`.
   - With the floor at Y = 0, set Position to `(0, 1, 0)`.

2. **Set up collision**
   - Remove Player’s **Capsule Collider** using its component menu.
   - Add Component → **Character Controller**.
   - Keep its defaults. Do not add a Rigidbody.

> **NOTE:** Unity’s Character Controller supplies the capsule’s collision handling

3. **Add the visor**
   - Create a Cube. Rename it `Visor`.
   - In the Hierarchy, drag it onto Player.
   - Set its local Position to `(0, 0.5, 0.5)`.
   - Set Rotation to `(0, 0, 0)`.
   - Set Scale to `(0.6, 0.2, 0.1)`.
   - Remove its **Box Collider**.
   
   > **NOTE:** The visor marks the capsule’s front: positive Z, and is really for the _developer's_ convenience, as players will never see it!

4. **Attach the camera**
   - In the Hierarchy, drag **Main Camera** onto Player.
   - Set its local Position to `(0, 0.6, 0)`.
   - Set Rotation to `(0, 0, 0)`.

5. **Save the scene**
   - Your floor needs a collider.
   - Next comes the script: movement, mouse look, and hiding the visor during play.

## Make the capsule controllable

1. **Enable input**
   - Edit → Project Settings → Player → Other Settings.
   - Set **Active Input Handling** to **Both**.
   - Restart Unity if prompted.

> **NOTE:** the "Both" setting enables the input calls used here; the script (below) supplies gravity for the Character Controller

2. **Create the script**
   - In the Project window, select your Assets folder.
   - Assets → Create → Scripting → MonoBehaviour Script.
   - Name it exactly `FirstPersonController`.

3. **Paste the code**
   - Double-click the script.
   - Replace everything with the complete code below.
   - Save, then return to Unity.

4. **Attach it**
   - Wait for Unity to finish compiling.
   - Drag the script onto **Player** in the Hierarchy.
   - Main Camera must still be a child of Player.

5. **Try it**
   - Press **Play**, then click inside the **Game** view.
   - **WASD / arrow keys:** walk.
   - **Mouse:** look around.
   - **Esc:** release the mouse. Click to resume.
   - Adjust **Move Speed** and **Look Sensitivity** in Player’s Inspector.

### Script

```csharp
using UnityEngine;

// Ensures this object has a Character Controller.
[RequireComponent(typeof(CharacterController))]
public class FirstPersonController : MonoBehaviour
{
    // These settings appear in the Inspector.
    public float moveSpeed = 4f;
    public float lookSensitivity = 2f;

    private CharacterController controller;
    private Transform eyes;
    private float pitch;          // Camera's up/down angle.
    private float verticalSpeed;  // Falling speed.

    void Start()
    {
        // Find the controller and the camera attached to Player.
        controller = GetComponent<CharacterController>();
        eyes = GetComponentInChildren<Camera>().transform;

        // Hide the capsule and visor during Play.
        // This only hides their appearance; collision still works.
        foreach (Renderer part in GetComponentsInChildren<Renderer>())
            part.enabled = false;
    }

    void Update()
    {
        // Escape releases the pointer; clicking captures it again.
        if (Input.GetKeyDown(KeyCode.Escape))
        {
            SetMouseLock(false);
            return;
        }

        if (Input.GetMouseButtonDown(0))
            SetMouseLock(true);

        // Pause control while the mouse is released.
        if (Cursor.lockState != CursorLockMode.Locked)
            return;

        // Turn the whole capsule left/right.
        float turn = Input.GetAxis("Mouse X") * lookSensitivity;
        transform.Rotate(0f, turn, 0f);

        // Tilt only the camera up/down; prevent flipping over.
        pitch -= Input.GetAxis("Mouse Y") * lookSensitivity;
        pitch = Mathf.Clamp(pitch, -85f, 85f);
        eyes.localRotation = Quaternion.Euler(pitch, 0f, 0f);

        // Read WASD / arrows and move relative to our facing direction.
        Vector3 movement =
            transform.right * Input.GetAxisRaw("Horizontal") +
            transform.forward * Input.GetAxisRaw("Vertical");

        // Prevent diagonal movement from being faster.
        movement = Vector3.ClampMagnitude(movement, 1f) * moveSpeed;

        // Apply gravity ourselves: Character Controller doesn't do it.
        if (controller.isGrounded && verticalSpeed < 0f)
            verticalSpeed = -2f;

        verticalSpeed -= 9.81f * Time.deltaTime;
        movement.y = verticalSpeed;

        // Move with collision; deltaTime makes speed independent of FPS.
        controller.Move(movement * Time.deltaTime);
    }

    void SetMouseLock(bool locked)
    {
        Cursor.lockState = locked
            ? CursorLockMode.Locked
            : CursorLockMode.None;

        Cursor.visible = !locked;
    }

    void OnDisable()
    {
        // Restore the pointer when this controller stops.
        SetMouseLock(false);
    }
}
```
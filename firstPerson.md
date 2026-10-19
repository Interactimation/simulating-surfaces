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
   - Check that your floor has a collider.
   - Next comes the script: movement, mouse look, and hiding the visor during play.

## Make the capsule controllable

> **NOTE:** the "Both" setting enables the input calls used here; the script (below) supplies gravity for the Character Controller

2. **Create the script**
   - In the Project window, select your Assets folder.
   - In the Assets Folder: Right Click → Create → Scripting → MonoBehaviour Script.
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
using UnityEngine.InputSystem; // Unity's new Input System.

[RequireComponent(typeof(CharacterController))]
public class FirstPersonController : MonoBehaviour
{
    // Adjust these in Player's Inspector.
    public float moveSpeed = 4f;
    public float mouseSensitivity = 0.1f; // Degrees per pixel of mouse movement.

    private CharacterController controller;
    private Transform eyes;
    private float pitch;          // Camera's up/down angle.
    private float verticalSpeed;  // Falling speed.

    void Start()
    {
        // Find Player's controller and child camera.
        controller = GetComponent<CharacterController>();
        eyes = GetComponentInChildren<Camera>().transform;

        // Hide the capsule and visor during Play.
        // Collision still works.
        foreach (Renderer part in GetComponentsInChildren<Renderer>())
            part.enabled = false;
    }

    void Update()
    {
        // Read devices through the new Input System.
        var keyboard = Keyboard.current;
        var mouse = Mouse.current;

        // Skip input if either device is unavailable.
        if (keyboard == null || mouse == null)
            return;

        // Escape releases the mouse.
        if (keyboard.escapeKey.wasPressedThisFrame)
        {
            SetMouseLock(false);
            return;
        }

        // Click to capture the mouse; begin control next frame.
        if (Cursor.lockState != CursorLockMode.Locked)
        {
            if (mouse.leftButton.wasPressedThisFrame)
                SetMouseLock(true);

            return;
        }

        // Mouse movement is measured in pixels since the last frame.
        // Do not multiply this by deltaTime.
        Vector2 look = mouse.delta.ReadValue() * mouseSensitivity;

        // Turn the whole capsule left/right.
        transform.Rotate(0f, look.x, 0f);

        // Tilt only the camera up/down; prevent flipping over.
        pitch = Mathf.Clamp(pitch - look.y, -85f, 85f);
        eyes.localRotation = Quaternion.Euler(pitch, 0f, 0f);

        // Read WASD and arrow keys.
        float x = 0f;
        float z = 0f;

        if (keyboard.aKey.isPressed || keyboard.leftArrowKey.isPressed)
            x -= 1f;
        if (keyboard.dKey.isPressed || keyboard.rightArrowKey.isPressed)
            x += 1f;
        if (keyboard.wKey.isPressed || keyboard.upArrowKey.isPressed)
            z += 1f;
        if (keyboard.sKey.isPressed || keyboard.downArrowKey.isPressed)
            z -= 1f;

        // Move relative to the direction Player faces.
        Vector3 movement = transform.right * x + transform.forward * z;

        // Prevent faster diagonal movement.
        movement = Vector3.ClampMagnitude(movement, 1f) * moveSpeed;

        // Apply gravity ourselves.
        if (controller.isGrounded && verticalSpeed < 0f)
            verticalSpeed = -2f;

        verticalSpeed -= 9.81f * Time.deltaTime;
        movement.y = verticalSpeed;

        // Move with collision, at the same speed regardless of FPS.
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
        // Restore the pointer when the controller stops.
        SetMouseLock(false);
    }
}
```

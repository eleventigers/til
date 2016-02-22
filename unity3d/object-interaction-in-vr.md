# Object interaction in VR

Interacting with 3D objects in VR can be very satisfying when paired with precise controller tracking. HTC Vive controllers track extremelly well and it is pretty easy to use them as pivots/anchors for any game object in a Unity3D scene.

When trying to implement a system to pick and move objects around, I struggled to come with a solution (due to the lack of gamedev experience, obviously) that worked with tracked controllers well. After a bit of digging through SteamVR developer forums, I stumbled upon [this thread](https://steamcommunity.com/app/358720/discussions/0/485622866452722018/) where a developer from [Fantastic Contraption](http://fantasticcontraption.com/) shared an insight:

> In FC, When pulling the trigger, we effectively parent the object to your hand, so it starts following your hand around. In Job Simulator, the object is instead attached with a ConfigurableJoint (if their OC2 talk is still relevant/accurate).

> In FC, we don't have physical colliders on the hands so they pass through objects that aren't being held. In Job Simulator you can "push" and egg off the table without holding down the trigger.

Building upon this suggestion I came up with:

```c#
[RequireComponent(typeof(SteamVR_TrackedObject))]
public class DraggableController : MonoBehaviour {

    public Rigidbody attachPoint;

    private SteamVR_TrackedObject trackedObj;

    private readonly HashSet<Collider> colliders = new HashSet<Collider> ();
    private readonly Dictionary<Collider, Joint> joints = new Dictionary<Collider, Joint> ();

    void Awake()
    {
        trackedObj = GetComponent<SteamVR_TrackedObject>();
    }

    void OnTriggerEnter(Collider other)
    {
        colliders.Add (other);
    }

    void OnTriggerExit(Collider other)
    {
        colliders.Remove (other);
    }

    void FixedUpdate()
    {
        var device = SteamVR_Controller.Input((int)trackedObj.index);
        if (device.GetTouchDown (SteamVR_Controller.ButtonMask.Trigger)) {
            ConnectJoints ();
        } else if (device.GetTouchUp (SteamVR_Controller.ButtonMask.Trigger)) {
            DisconnectJoints ();
        }
    }

    private void ConnectJoints()
    {
        foreach (Collider collider in colliders) {
            ConnectJoint (collider);
        }
    }

    private void DisconnectJoints()
    {
        foreach (Collider collider in colliders) {
            DisconnectJoint (collider);
        }
    }

    private void ConnectJoint(Collider collider)
    {
        if (!joints.ContainsKey (collider)) {
            var joint = collider.gameObject.AddComponent<FixedJoint> ();
            joint.connectedBody = attachPoint;
            joints.Add (collider, joint);
        }
    }

    private void DisconnectJoint(Collider collider)
    {
        if (joints.ContainsKey(collider)) {
            var joint = joints [collider];
            var go = joint.gameObject;
            Object.DestroyImmediate (joint);
            joints.Remove (collider);
        }
    }
}
```
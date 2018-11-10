# AR Workshop

## Step 1
Create a new project that targets your desired platflorm

## Step 2

Import packages from the packet manager

* ARKit XR Plugin
* ARCore XR Plugin
* ARFoundation 

## Step 3

Edit **Project Settings > Player** and add **CameraUsageDescription**

Create `link.xml`

```xml
<linker>
    <assembly fullname="Unity.XR.ARCore" ignoreIfMissing="1" preserve="all"/>
    <assembly fullname="Unity.XR.ARKit" ignoreIfMissing="1" preserve="all"/>
</linker>
```

## Step 4

Set up scene

* AR Session
* AR Session Origin
  * AR Plane Manager
  * AR Point Cloud Manager

## Step 5

```csharp
void Start () {
	PlaneManager.planeAdded += PlaneManagerOnPlaneManagerAdded;
}

private void PlaneManagerOnPlaneManagerAdded(ARPlaneAddedEventArgs obj)
{
	var go = Instantiate(particlePrefab, obj.plane.transform);
	go.transform.localPosition = Vector3.zero;
	go.transform.localScale = Vector3.one;
}
```

## Step 6

Minigame script

```csharp
public class Minigame : MonoBehaviour
{
	public GameObject prefab;
	private int level;
	public List<GameObject> objects;
	public float size = 0.2f;

	void Awake()
	{
		level = 1;
		StartCoroutine(Game());	
	}

	void Update()
	{
		if (Input.touchCount > 0)
		{
			Touch touch = Input.GetTouch(0);
			var ray = Camera.main.ScreenPointToRay(touch.position);
			RaycastHit hit;
			if (Physics.Raycast(ray, out hit))
			{
				if (objects.Contains(hit.collider.gameObject))
				{
					Destroy(hit.collider.gameObject);
				}
			}
		}
	}


	IEnumerator Game()
	{
		for (int i = 0; i < level; i++)
		{
			var obj = Instantiate(prefab, transform);
			obj.transform.localPosition = Vector3.forward * Random.Range(-size, size) + Vector3.left * Random.Range(-size, size);
			obj.transform.localRotation = transform.rotation;
			objects.Add(obj);
		}

		yield return new WaitForSeconds(5f);
		level++;
		StartCoroutine(Game());
	}
}
```

## Issues building for Android

search here https://github.com/Unity-Technologies/arfoundation-samples/issues

using UnityEngine;
using UnityEngine.Rendering;

public class PlayerAABB : MonoBehaviour 
{
    [Header("Setup")]
    public Transform target;
    public float speed = 5f;

    private Renderer _me;
    private Renderer _target;
    private bool _intersects;

    void Awake()
    {
        _me = GetComponent<Renderer>();
        _target = target.GetComponent<Renderer>();
    }

    void Update()
    {
       float h = Input.GetAxis("Horizontal");
        float v = Input.GetAxis("Vertical");
        Vector3 move = new Vector3(h, 0, v) * speed * Time.deltaTime;
        transform.Translate(move, Space.World);

        _intersects = _me.bounds.Intersects(_target.bounds);

        Bounds a = _me.bounds;
        Bounds b = _target.bounds;
        bool manual =
            (a.min.x <= b.max.x && a.max.x >= b.min.x) &&
            (a.min.y <= b.max.y && a.max.y >= b.min.y) &&
            (a.min.z <= b.max.z && a.max.z >= b.min.z);
        if (_intersects != manual)
        {
            Debug.LogWarning("Bounds.Intersects and manual AABB disagree--check setup!");
        }
        if (_intersects)
        {
            Debug.Log("Intersecting!");
        }

        void OnDrawGizmos()
        {
            if ( _me != null )
            Gizmos.DrawWireCube(_me.bounds.center, _me.bounds.size);
            if (_target != null)
            {
                var tr = target.GetComponent<Renderer>();
                if( tr != null)
                    Gizmos.DrawWireCube(tr.bounds.center, tr.bounds.size);
            }

        }

    }
}



# MAT102_Assessment3_Banki_Balazs_CollisionDetection

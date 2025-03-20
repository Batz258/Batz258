using UnityEngine;
using System.Collections;

public class CarController : MonoBehaviour
{
    public float acceleration = 500f;
    public float maxSpeed = 50f;
    public float turnSpeed = 5f;
    public float jumpForce = 5f;
    public float boostForce = 1000f;
    public float boostDuration = 2f;

    private Rigidbody rb;
    private bool isGrounded;
    private bool isBoosting = false;

    void Start()
    {
        rb = GetComponent<Rigidbody>();
    }

    void Update()
    {
        // Хөдөлгөөн
        float moveInput = Input.GetAxis("Vertical");
        rb.AddForce(transform.forward * moveInput * acceleration * Time.deltaTime, ForceMode.Acceleration);
        rb.velocity = Vector3.ClampMagnitude(rb.velocity, maxSpeed);

        // Эргэлт
        float turnInput = Input.GetAxis("Horizontal");
        transform.Rotate(Vector3.up * turnInput * turnSpeed);

        // Үсрэлт
        if (Input.GetKeyDown(KeyCode.Space) && isGrounded)
        {
            rb.AddForce(Vector3.up * jumpForce, ForceMode.Impulse);
            isGrounded = false;
        }

        // Nitro Boost
        if (Input.GetKeyDown(KeyCode.LeftShift) && !isBoosting)
        {
            StartCoroutine(Boost());
        }
    }

    void OnCollisionEnter(Collision collision)
    {
        if (collision.gameObject.CompareTag("Ground"))
        {
            isGrounded = true;
        }
    }

    IEnumerator Boost()
    {
        isBoosting = true;
        rb.AddForce(transform.forward * boostForce, ForceMode.Impulse);
        yield return new WaitForSeconds(boostDuration);
        isBoosting = false;
    }
}

<!---
Batz258/Batz258 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->

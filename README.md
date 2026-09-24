# Git-Aktivated
[ Bantuan ] [ Peraturan ] [ Report ]
[ Spawn  ] [ Starter Kit ] [ Tutup ]
using UnityEngine;

public class PlayerController : MonoBehaviour
{
    public float moveSpeed = 5f;
    public float jumpForce = 7f;
    public Rigidbody rb;

    private bool isGrounded;

    void Start()
    {
        if (rb == null)
            rb = GetComponent<Rigidbody>();
    }

    void Update()
    {
        float moveX = Input.GetAxis("Horizontal");
        float moveZ = Input.GetAxis("Vertical");

        Vector3 movement = new Vector3(moveX, 0f, moveZ) * moveSpeed;
        rb.velocity = new Vector3(movement.x, rb.velocity.y, movement.z);

        if (Input.GetKeyDown(KeyCode.Space) && isGrounded)
        {
            rb.velocity = new Vector3(rb.velocity.x, jumpForce, rb.velocity.z);
            isGrounded = false;
        }
    }

    void OnCollisionStay(Collision collision)
    {
        if (collision.gameObject.CompareTag("Ground"))
        {
            isGrounded = true;
        }
    }
}
data class PlayerInfo(
    val name: String,
    val isAdmin: Boolean
)

class AdminPanelActivity : AppCompatActivity() {

    private val players = listOf(
        PlayerInfo("PlayerA", false),
        PlayerInfo("PlayerB", true),
        PlayerInfo("PlayerC", false)
    )

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_admin_panel)

        val adminName = "owner" // contoh
        val currentUser = "owner"

        if (currentUser != adminName) {
            showToast("Akses ditolak. Hanya admin yang bisa membuka panel ini.")
            finish()
            return
        }

        // Contoh daftar pemain
        val listView = findViewById<ListView>(R.id.playerList)
        val adapter = ArrayAdapter(this, android.R.layout.simple_list_item_1, players.map { it.name })
        listView.adapter = adapter

        listView.setOnItemClickListener { _, _, position, _ ->
            val player = players[position]
            showDialog(player)
        }
    }

    private fun showDialog(player: PlayerInfo) {
        val actions = arrayOf("Kick", "Ban", "Mute", "Teleport ke Spawn", "Beri Item Resmi")
        val builder = AlertDialog.Builder(this)
        builder.setTitle("Aksi untuk ${player.name}")
        builder.setItems(actions) { _, which ->
            when (which) {
                0 -> showToast("Kick ${player.name}")
                1 -> showToast("Ban ${player.name}")
                2 -> showToast("Mute ${player.name}")
                3 -> showToast("Teleport ${player.name} ke spawn")
                4 -> showToast("Memberi item resmi ke ${player.name}")
            }
        }
        builder.show()
    }

    private fun showToast(message: String) {
        Toast.makeText(this, message, Toast.LENGTH_SHORT).show()
    }
}
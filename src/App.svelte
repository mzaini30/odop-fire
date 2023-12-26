<script>
  import gosit from "gosit";
  let hasil = [];
  let banyak = 0;
  async function ambil_data() {
    let inputData = await gosit(
      "1AmwL4HHSCuJiX7Lykp7i6dyWG4GgijfI-2X8qAKfU6M",
      "Form Responses 1"
    );
    banyak = inputData.length;

    // Objek untuk menyimpan hasil transformasi
    var transformedData = [
      {
        title: "Pilihan",
        data: [],
      },
      {
        title: "ODOP Angkatan",
        data: [],
      },
    ];

    // Membuat objek untuk menyimpan data sementara
    var odopAngkatan = {};
    var pilihan = {};

    // Iterasi melalui data input
    inputData.forEach(function (entry) {
      var angkatan = entry["Kamu ODOP Angkatan berapa? "];
      var pemimpin =
        entry[
          "Dari 5 nama berikut, siapa yang paling Anda percaya bisa memimpin ODOP selama 2 tahun ke depan? "
        ];

      // Menghitung jumlah angkatan
      odopAngkatan[angkatan] = (odopAngkatan[angkatan] || 0) + 1;

      // Menghitung jumlah pemimpin
      pilihan[pemimpin] = (pilihan[pemimpin] || 0) + 1;
    });

    // Transformasi data pilihan
    for (var angkatan in odopAngkatan) {
      transformedData[1].data.push({
        title: angkatan,
        quantity: odopAngkatan[angkatan],
      });
    }

    // Transformasi data angkatan
    for (var pemimpin in pilihan) {
      transformedData[0].data.push({
        title: pemimpin.split(" ")[0],
        quantity: pilihan[pemimpin],
      });
    }

    // kode sebelumnya

    // Mencari nilai maksimum untuk ODOP Angkatan
    var maxAngkatan = Math.max(
      ...transformedData[0].data.map((item) => item.quantity)
    );

    // Menandai data angkatan dengan kuantitas paling banyak
    transformedData[0].data.forEach((item) => {
      item.highlight = item.quantity === maxAngkatan;
    });

    // Mencari nilai maksimum untuk Pilihan
    var maxPilihan = Math.max(
      ...transformedData[1].data.map((item) => item.quantity)
    );

    // Menandai data pilihan dengan kuantitas paling banyak
    transformedData[1].data.forEach((item) => {
      item.highlight = item.quantity === maxPilihan;
    });

    // Menampilkan hasil transformasi dengan penyorotan
    console.log(transformedData);

    // Menampilkan hasil transformasi
    // console.log(transformedData);
    hasil = transformedData;
  }
  ambil_data();
</script>

<div class="px-[10%] py-3 bg-gray-200">
  <button class="btn mb-3">
    <div class="badge">{banyak}</div>
    Pemilih
  </button>
  <div class="grid grid-cols-1 sm:grid-cols-2 gap-3">
    {#each hasil as x}
      <div class="card w-full bg-base-100 shadow-xl">
        <div class="card-body">
          <h2 class="card-title">{x.title}</h2>
          <div class="overflow-x-auto">
            <table class="table">
              <tbody>
                <!-- row 1 -->
                {#each x.data as y}
                  <tr class={y.highlight ? "bg-base-200" : null}>
                    <th>{y.title}</th>
                    <td>{y.quantity} Pemilih</td>
                  </tr>
                {/each}
              </tbody>
            </table>
          </div>
        </div>
      </div>
    {/each}
  </div>
</div>

<svelte:head>
  <title>ODOP Fire 🔥</title>
</svelte:head>

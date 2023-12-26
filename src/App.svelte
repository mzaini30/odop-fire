<script>
  import gosit from "gosit";
  let hasil = [];
  async function ambil_data() {
    let inputData = await gosit(
      "1AmwL4HHSCuJiX7Lykp7i6dyWG4GgijfI-2X8qAKfU6M",
      "Form Responses 1"
    );

    // Objek untuk menyimpan hasil transformasi
    var transformedData = [
      {
        title: "ODOP Angkatan",
        data: [],
      },
      {
        title: "Pilihan",
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

    // Transformasi data angkatan
    for (var angkatan in odopAngkatan) {
      transformedData[0].data.push({
        title: angkatan,
        quantity: odopAngkatan[angkatan],
      });
    }

    // Transformasi data pilihan
    for (var pemimpin in pilihan) {
      transformedData[1].data.push({
        title: pemimpin.split(" ")[0],
        quantity: pilihan[pemimpin],
      });
    }

    // Menampilkan hasil transformasi
    // console.log(transformedData);
    hasil = transformedData;
  }
  ambil_data();
</script>

<pre>{JSON.stringify(hasil, null, 2)}</pre>

<svelte:head>
  <title>ODOP Fire 🔥</title>
</svelte:head>

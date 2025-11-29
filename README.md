# Uzii
Jasa suruhimport React, { useState } from "react";

// Single-file React component for a simple, modern "Jasa Suruh" (errand service) landing + booking form
// Tailwind CSS classes are used for styling (no imports needed here).
// Uses shadcn/ui style components as optional placeholders (import lines kept for integration).

import { Button } from "@/components/ui/button";
import { Card, CardContent } from "@/components/ui/card";
import { Input } from "@/components/ui/input";
import { Textarea } from "@/components/ui/textarea";

export default function JasaSuruhApp() {
  const [form, setForm] = useState({
    name: "uzii",
    phone: "085374535285",
    address: "",
    task: "",
    date: "",
    notes: "",
  });
  const [status, setStatus] = useState(null);

  function handleChange(e) {
    const { name, value } = e.target;
    setForm((s) => ({ ...s, [name]: value }));
  }

  async function handleSubmit(e) {
    e.preventDefault();
    setStatus("sending");

    // Placeholder: integrate with your backend / Airtable / Google Forms / WhatsApp API
    // For now we'll open the user's WhatsApp with a prefilled message as a quick live demo.
    const msg = `Halo, saya ${encodeURIComponent(
      form.name || "(nama)"
    )} ingin meminta bantuan: ${encodeURIComponent(form.task || "(deskripsi)")}.\nAlamat: ${encodeURIComponent(
      form.address || "(alamat)"
    )}\nTanggal: ${encodeURIComponent(form.date || "segera")}\nCatatan: ${encodeURIComponent(form.notes || "-")}`;

    // WhatsApp demo link (works if user opens on phone/has WhatsApp web)
    const wa = `https://wa.me/?text=${msg}`;

    try {
      window.open(wa, "_blank");
      setStatus("sent");
    } catch (err) {
      console.error(err);
      setStatus("error");
    }
  }

  const services = [
    { title: "Antar & Jemput", desc: "Kirim paket, ambil barang, atau antar dokumen cepat." },
    { title: "Belanja & Pembelian", desc: "Belanja kebutuhan harian atau barang khusus.", },
    { title: "Pembayaran Tagihan", desc: "Bayar listrik, air, atau top-up e-wallet.", },
    { title: "Cek & Administrasi", desc: "Antar berkas, cek layanan, atau urus keperluan kantor." },
  ];

  return (
    <div className="min-h-screen bg-gradient-to-b from-white to-slate-50 text-slate-900">
      <header className="max-w-5xl mx-auto p-6 flex items-center justify-between">
        <div className="flex items-center gap-3">
          <div className="w-12 h-12 rounded-2xl bg-slate-800 flex items-center justify-center text-white text-lg font-bold">JS</div>
          <div>
            <h1 className="text-xl font-semibold">JasaSuruh</h1>
            <p className="text-sm text-slate-500">Bantuan cepat — belanja, antar, bayar, urus.</p>
          </div>
        </div>
        <nav className="hidden md:flex gap-4 items-center">
          <a href="#services" className="text-sm hover:underline">Layanan</a>
          <a href="#pricing" className="text-sm hover:underline">Harga</a>
          <a href="#booking" className="text-sm hover:underline">Booking</a>
          <a href="#contact" className="text-sm hover:underline">Kontak</a>
        </nav>
        <div className="md:hidden text-sm text-slate-600">Hub: +62 8xx</div>
      </header>

      <main className="max-w-5xl mx-auto px-6 pb-28">
        <section className="grid md:grid-cols-2 gap-8 items-center mt-6">
          <div>
            <h2 className="text-3xl md:text-4xl font-extrabold leading-tight">Butuh bantuan sehari-hari?<br/>Kami siap bantu.</h2>
            <p className="mt-4 text-slate-600">Pesan layanan antar, belanja, bayar tagihan, atau urus keperluan lain — cepat, terpercaya, dan transparan.</p>

            <div className="mt-6 flex gap-3 flex-wrap">
              <a href="#booking" className="rounded-xl border border-slate-800 px-4 py-2 font-medium">Pesan Sekarang</a>
              <a href="#services" className="rounded-xl px-4 py-2 bg-slate-800 text-white font-medium">Lihat Layanan</a>
            </div>

            <div className="mt-6 grid grid-cols-2 gap-3">
              <div className="p-4 rounded-xl bg-white shadow-sm">
                <div className="text-sm text-slate-500">Mulai dari</div>
                <div className="text-xl font-semibold mt-1">Rp 15.000</div>
                <div className="text-xs text-slate-400">(lokal / jarak dekat)</div>
              </div>
              <div className="p-4 rounded-xl bg-white shadow-sm">
                <div className="text-sm text-slate-500">Waktu respons</div>
                <div className="text-xl font-semibold mt-1">~30 menit</div>
                <div className="text-xs text-slate-400">(tergantung lokasi)</div>
              </div>
            </div>
          </div>

          <div className="bg-white rounded-2xl p-6 shadow-md">
            <h3 className="text-lg font-semibold">Coba pesan via WhatsApp (demo)</h3>
            <p className="text-sm text-slate-500 mt-1">Isi form cepat dan kami akan menghubungi Anda lewat WhatsApp.</p>

            <form id="booking" onSubmit={handleSubmit} className="mt-4 space-y-3">
              <Input name="name" placeholder="Nama lengkap" value={form.name} onChange={handleChange} />
              <Input name="phone" placeholder="Nomor HP (WhatsApp)" value={form.phone} onChange={handleChange} />
              <Input name="address" placeholder="Alamat / titik jemput" value={form.address} onChange={handleChange} />
              <Input name="date" type="date" placeholder="Tanggal" value={form.date} onChange={handleChange} />
              <Textarea name="task" placeholder="Deskripsi tugas (mis. beli 2 roti, antar ke...)" value={form.task} onChange={handleChange} />
              <Textarea name="notes" placeholder="Catatan tambahan (opsional)" value={form.notes} onChange={handleChange} />

              <div className="flex gap-2">
                <Button type="submit">Kirim ke WhatsApp</Button>
                <button type="button" onClick={() => setForm({ name: "", phone: "", address: "", task: "", date: "", notes: "" })} className="px-4 py-2 rounded-xl border">Reset</button>
              </div>

              {status === "sending" && <div className="text-sm text-slate-500">Membuka WhatsApp...</div>}
              {status === "sent" && <div className="text-sm text-green-600">Link WhatsApp dibuka — lanjutkan pesan di aplikasi.</div>}
              {status === "error" && <div className="text-sm text-red-600">Terjadi kesalahan. Coba lagi.</div>}
            </form>
          </div>
        </section>

        <section id="services" className="mt-12">
          <h3 className="text-2xl font-semibold">Layanan Kami</h3>
          <p className="text-slate-600 mt-2">Pilih layanan yang Anda butuhkan. Harga bisa berubah berdasarkan jarak dan kompleksitas.</p>

          <div className="grid sm:grid-cols-2 lg:grid-cols-4 gap-6 mt-6">
            {services.map((s) => (
              <Card key={s.title} className="p-4">
                <CardContent>
                  <h4 className="font-semibold">{s.title}</h4>
                  <p className="text-sm text-slate-500 mt-1">{s.desc}</p>
                </CardContent>
              </Card>
            ))}
          </div>
        </section>

        <section id="pricing" className="mt-12">
          <h3 className="text-2xl font-semibold">Contoh Harga</h3>
          <div className="mt-4 grid sm:grid-cols-2 gap-4">
            <div className="p-4 bg-white rounded-xl shadow-sm">
              <div className="text-sm text-slate-500">Jarak dekat (≤5 km)</div>
              <div className="text-xl font-bold mt-1">Rp 15.000 — Rp 30.000</div>
            </div>
            <div className="p-4 bg-white rounded-xl shadow-sm">
              <div className="text-sm text-slate-500">Jarak menengah (5–15 km)</div>
              <div className="text-xl font-bold mt-1">Rp 30.000 — Rp 60.000</div>
            </div>
          </div>
          <p className="text-xs text-slate-400 mt-3">Catatan: Harga final ditentukan setelah konfirmasi lokasi dan kondisi tugas.</p>
        </section>

        <section className="mt-12 grid md:grid-cols-2 gap-6 items-start">
          <div>
            <h3 className="text-2xl font-semibold">Testimoni</h3>
            <div className="mt-4 space-y-3">
              <div className="p-4 bg-white rounded-xl shadow-sm">
                <div className="font-medium">Siti, Solo</div>
                <div className="text-sm text-slate-600 mt-1">Cepat dan bantu belanja hari itu juga. Recommended!</div>
              </div>
              <div className="p-4 bg-white rounded-xl shadow-sm">
                <div className="font-medium">Budi, Karanganyar</div>
                <div className="text-sm text-slate-600 mt-1">Pengemudi ramah dan tepat waktu.</div>
              </div>
            </div>
          </div>

          <div id="contact" className="p-4 bg-white rounded-xl shadow-sm">
            <h4 className="font-semibold">Kontak</h4>
            <p className="text-sm text-slate-600 mt-2">Whatsapp: +62 8xx-xxxx-xxxx</p>
            <p className="text-sm text-slate-600">Email: admin@jasasuruh.example</p>

            <div className="mt-4">
              <iframe title="map" className="w-full h-40 rounded" src="https://www.google.com/maps/embed?pb=!1m18!" />
            </div>
          </div>
        </section>
      </main>

      <footer className="border-t bg-white mt-12 py-6">
        <div className="max-w-5xl mx-auto px-6 flex flex-col md:flex-row justify-between items-center gap-3">
          <div className="text-sm text-slate-600">© {new Date().getFullYear()} JasaSuruh — Semua hak dilindungi.</div>
          <div className="text-sm text-slate-600">Syarat & Ketentuan | Kebijakan Privasi</div>
        </div>
      </footer>
    </div>
  );
}


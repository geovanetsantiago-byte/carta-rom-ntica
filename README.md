import React, { useState, useRef, useEffect } from "react"; import { Card, CardContent } from "@/components/ui/card"; import { Button } from "@/components/ui/button"; import { motion } from "framer-motion";

export default function CartaRomanticaSite() { const [letter, setLetter] = useState(""); const [images, setImages] = useState([]); const [music, setMusic] = useState(""); const containerRef = useRef(null); const audioRef = useRef(null);

// Upload até 4 fotos const handleImages = (e) => { const files = Array.from(e.target.files).slice(0, 4); const previews = files.map((file) => URL.createObjectURL(file)); setImages(previews); };

// Música selecionada const handleMusic = (e) => { const file = e.target.files[0]; if (file) { const url = URL.createObjectURL(file); setMusic(url); } };

// Efeito de pétalas e corações useEffect(() => { const container = containerRef.current; if (!container) return;

const createFallingItem = () => {
  const el = document.createElement("div");
  el.innerText = Math.random() > 0.5 ? "❤" : "🌸";
  el.style.position = "absolute";
  el.style.left = Math.random() * 100 + "%";
  el.style.top = "-20px";
  el.style.fontSize = Math.random() * 20 + 20 + "px";
  el.style.opacity = 0.8;
  el.style.animation = `fall ${5 + Math.random() * 5}s linear forwards`;
  container.appendChild(el);

  setTimeout(() => el.remove(), 10000);
};

const interval = setInterval(createFallingItem, 500);
return () => clearInterval(interval);

}, []);

// Tocar música automaticamente useEffect(() => { if (audioRef.current && music) { audioRef.current.play().catch(() => {}); } }, [music]);

return ( <div className="min-h-screen bg-black text-white flex flex-col items-center p-6 relative overflow-hidden" ref={containerRef}> <style>{@keyframes fall { 0% { transform: translateY(0) rotate(0deg); opacity: 1; } 100% { transform: translateY(110vh) rotate(360deg); opacity: 0; } }}</style>

<motion.h1
    initial={{ opacity: 0, y: -20 }}
    animate={{ opacity: 1, y: 0 }}
    className="text-3xl font-bold mb-6 text-center"
  >
    💌 Carta Romântica
  </motion.h1>

  <Card className="w-full max-w-2xl bg-neutral-900 border-neutral-700">
    <CardContent className="space-y-4 p-6">
      <textarea
        placeholder="Escreva sua carta aqui..."
        value={letter}
        onChange={(e) => setLetter(e.target.value)}
        className="w-full h-40 p-3 rounded-xl bg-black border border-neutral-700 text-white resize-none"
      />

      <div>
        <p className="mb-2">Adicionar até 4 fotos:</p>
        <input type="file" accept="image/*" multiple onChange={handleImages} />
      </div>

      <div>
        <p className="mb-2">Escolher música:</p>
        <input type="file" accept="audio/*" onChange={handleMusic} />
      </div>

      {music && <audio ref={audioRef} src={music} loop />}
    </CardContent>
  </Card>

  {/* Galeria */}
  <div className="grid grid-cols-2 gap-4 mt-6 max-w-2xl w-full">
    {images.map((img, i) => (
      <motion.img
        key={i}
        src={img}
        alt="foto"
        className="rounded-2xl shadow-lg object-cover w-full h-48"
        initial={{ opacity: 0, scale: 0.8 }}
        animate={{ opacity: 1, scale: 1 }}
      />
    ))}
  </div>

  {/* Visualização da carta */}
  {letter && (
    <motion.div
      initial={{ opacity: 0 }}
      animate={{ opacity: 1 }}
      className="mt-8 max-w-2xl text-center whitespace-pre-wrap text-lg leading-relaxed"
    >
      {letter}
    </motion.div>
  )}

  <div className="mt-10 text-sm text-neutral-400">
    Feito com ❤️
  </div>
</div>

); }

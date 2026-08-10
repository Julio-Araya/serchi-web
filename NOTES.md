# NOTES — deuda conocida y decisiones pendientes

## DNS
- `serchi.ai` está en Cloudflare, hoy apuntando a Lovable. Hay que moverlo a
  Vercel en modo **DNS-only (nube gris), no proxied**.

## Legal
- **No existe Política de Privacidad en ninguna parte.** Sí existen Términos
  de Servicio, Política de Reembolsos y Seguridad (rescatados en
  `content/copy-legacy.md`). Es un vacío legal y **bloquea la decisión de
  cookies/analytics**.

## Analytics
- v1 usará **Vercel Web Analytics (cookieless)**. Sin GA4, sin píxeles, sin
  banner de consentimiento.

## Arquitectura
- El sitio es **100 % estático**. Sin backend, sin Supabase, sin formularios
  que posteen a ningún lado. El CTA de agendar apunta a un link externo de
  Calendly (**URL pendiente** — la landing vieja usaba
  `https://calendly.com/julio-araya-palacios/30min`).

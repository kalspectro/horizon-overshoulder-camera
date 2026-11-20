# Continuidade do OverShoulder Camera

## Alvo
- Horizon Nightly de 13/11/2026
- Build `native-9f42064-dac394f91015`
- Horizon revision `9f42064c11efe7ef3e5ed2baad7ddc2a3b36eea0`
- Game version `26.3`
- Formato nativo `.hnmod`

## Objetivo
Câmera próxima atrás do jogador, ligeiramente deslocada para o ombro direito, inspirada em jogos de ação em terceira pessoa. A câmera deve manter o personagem visível e oferecer movimento suave e mira central.

## Estado do protótipo v0.1.0
- Mod ID: `overshoulder`
- Tecla F6 para alternar.
- Deslocamento aplicado à pose: +2.15 no forward, +0.38 no right e +0.10 vertical.
- Yaw e pitch preservados.
- Suavização exponencial usando `thread_local!`, `RefCell` e `Instant`.
- Mira central desenhada no HUD.

## APIs confirmadas no Atlas 0.3.0
- `native_camera_pose_now` RETURN com `(glam::DVec3, f32, f32)`.
- `native_key(game, key, pressed)`.
- `native_mods(mods)`.
- `native_hud(game, batch, corners)`.
- `game.native_fill(batch, [x,y,w,h], argb)`.
- `game.native_playing()`, `game.native_on(id)`, `game.native_mod_key(...)`, `game.native_chat(...)`.

Convenção yaw: yaw 0 aponta para +Z; forward `[-sin(yaw)*cos(pitch), -sin(pitch), cos(yaw)*cos(pitch)]`; right `[-cos(yaw),0,-sin(yaw)]`.

## Limitações conhecidas
A mira é apenas visual. Não há raycast, alinhamento de projéteis, colisão da câmera com blocos ou detecção automática do modo primeira/terceira pessoa. O `camera.rs` do Atlas registra a pose, mas não expõe controle direto da câmera.

## Validação
O pacote foi validado quanto à integridade ZIP e parsing JSON/TOML. Não foi compilado nem testado no runtime Horizon; não afirmar que está plenamente funcional. `rustfmt` e compilador/runtime Horizon não estavam disponíveis no ambiente de criação.

## Próximos passos
1. Confirmar a API real de interceptação da pose da câmera no build-alvo.
2. Compilar o `.hnmod` com o toolchain compatível com Horizon Nightly.
3. Testar F6, suavização e posição em jogo.
4. Implementar colisão por raycast e ajustar mira/projéteis somente após confirmar APIs suportadas.
5. Detectar primeira/terceira pessoa e aplicar offset apenas no modo pretendido.

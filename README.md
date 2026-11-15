# muzikale-models

On-device machine-learning models downloaded by the **Muzikale** app (iPhone, iPad and Mac) for its optional
"Qualidade estúdio" (studio-quality) instrument separation. The app downloads them over HTTPS from this
repository's releases, checks their SHA-256, unpacks them and compiles them on the device. Nothing here is
needed to use the app's default (fast) separation.

## Release v1

| File | Model | Size | SHA-256 |
|---|---|---|---|
| `MelBandRoformerVocals.aar` | Mel-Band RoFormer, vocals (Kim) — Core ML, int8 weights | 206 MB (205,962,655 bytes) | `047e830adf32a9cad7755ae2bf1178a82b19350febe122a56dbd965e159ea061` |

`MelBandRoformerVocals.aar` is an Apple Archive (LZFSE) holding `MelBandRoformerVocals.mlpackage`
(Core ML ML Program, iOS 18 / macOS 15 or later).

- Input `spec` `[1, 2, 2, 1025, 801]` float32: the STFT of an 8-second stereo chunk at 44.1 kHz
  (channel, real/imaginary, bin, frame; n_fft 2048, hop 441, periodic Hann window, `center=True` with reflect
  padding, unnormalised — `torch.stft` conventions).
- Output `vocals`, same shape: the masked spectrum of the vocals. The app runs the inverse STFT, blends
  half-overlapping chunks, and splits the rest of the song (song − vocals) with htdemucs_6s.

## Credits

- **Weights**: Kimberley Jensen ([KimberleyJSN/melbandroformer](https://huggingface.co/KimberleyJSN/melbandroformer)
  on Hugging Face, `MelBandRoformer.ckpt`), MIT license. Converted to Core ML without retraining (format change,
  float16 compute, 8-bit linear weight quantization; measured equal to float16 within 0.01 dB SDR).
- **Architecture**: Ju-Chiang Wang, Wei-Tsung Lu, Minz Won — *Mel-Band RoFormer for Music Source Separation*
  (2023).
- **Model code used for the conversion**: [ZFTurbo/Music-Source-Separation-Training](https://github.com/ZFTurbo/Music-Source-Separation-Training)
  (MIT, Roman Solovyev), after [lucidrains/BS-RoFormer](https://github.com/lucidrains/BS-RoFormer) (MIT,
  Phil Wang).

## License

MIT — see [LICENSE](LICENSE) (the weights' license and credits).

## Stack notes — Shader / GPU

### Toolchain & commands
- Language {{shader_lang}} (WGSL/GLSL/HLSL); target APIs {{gpu_apis}}; host environment
  {{shader_host}} (engine material system / standalone). Hot reload: {{hot_reload}}.

### Agent failure modes
- Verifying by reading code — a shader is proven by rendering it; every change gets its
  output looked at ({{render_check}}), not inferred.
- Binding/uniform layout drift between shader and host code — the two move in the same
  change ({{binding_source}} is the source of truth; regenerate, don't parallel-edit).
- Precision assumptions: what works in highp on desktop banding/breaking at mediump on
  mobile; NDC/UV y-flip and depth-range differences between {{gpu_apis}}.
- Magic numbers for tweakables — thresholds and constants get names (and a uniform, if
  they're meant to be tuned).
- Perf claims without a GPU capture — branch cost, occupancy, and bandwidth are measured
  ({{gpu_profiler}}), not guessed from instruction count.

### Verification norm
Proven = compiles for every target API + rendered output checked against expectation
({{golden_policy}}); perf-sensitive changes come with a capture.

### Interview add-ons
- Language + target APIs? (infer: file extensions, naga/tint/glslang configs)
- Host binding mechanism — codegen, reflection, hand-written? (infer: build scripts)
- Golden-image tests or eyeball-with-screenshot? (infer: test dirs)
- GPU profiler available in this workflow? (renderdoc/Xcode/PIX)

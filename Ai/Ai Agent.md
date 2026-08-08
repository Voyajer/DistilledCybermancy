#What this is:
This is an overview of how to get a basic local LLM agent and harness working for VSCode.

#Setting up an Ai Agent:
#What goes into an Ai Agent:
An Ai agent is really a combination of multiple smaller components, the model, the backend, and the harness. The model is the actual vectorized data that the backend uses on the inputs you provide, the backend is the software that calculates the result from running the input tokens through the model, and the harness is what interperets the results of the output tokens to do actual work like modifying files or executing commands.

#Software that will be used:
Replacating my setup, we'll be using Zoo Code as the harness for its local LLM support, LMStudio (wrapping Llama.cpp) as the backend that loads the model and communicates with Zoo, for its convenient GUI for tuning parameters, and a Qwen-3.6-27B variant for its space efficiency and coding ability.

#Install:
VSCode, Zoo Code within VSCode, and LMStudio. Optionally nvtop on Linux can be useful for tracking VRAM usage for when we start turning the model.

#Download:
An appropriate Qwen 3.6 (or newer) model that pairs well with your GPU's VRAM amount from https://huggingface.co/unsloth/Qwen3.6-27B-MTP-GGUF.
I have a 24GB GPU so I chose Qwen3.6-27B-UD-Q4_K_XL.gguf. It's 18GB large so it will definitely fit into my VRAM even with other things running while having enough room for context fill. For a 16GB card I would probably choose either Qwen3.6-27B-UD-IQ3_XXS.gguf (12.2GB) or Qwen3.5-9B-Q8_0.gguf (9.53GB) from https://huggingface.co/unsloth/Qwen3.5-9B-GGUF. (Useful resource at https://unsloth.ai/docs/models/qwen3.5)

#Configuration:
Open LMStudio, on the left bar click on "my models", then in the bottom right click on the three dots (...) button and select Change... . Determine and select where you want to store your LLM models. Mine are at ~/Documents/ai/models/. We'll move the model into here later.

Next click the settings cog in the bottom left of the window and click on the runtime tab. Here make sure an appropriate llama.cpp variant is installed. Vulkan works but GPU specific should be faster. Set the preferred variant at the top.


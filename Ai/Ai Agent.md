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
#LMStudio:b
Open LMStudio, on the left bar click on "my models", then in the bottom right click on the three dots (...) button and select Change... . Determine and select where you want to store your LLM models. Mine are at ~/Documents/ai/models/. Move your model into this folder EXCEPT, it must be nested within where you got it, and what it's called (/huggingface/qwen3.x-xxxxxxx/)

Next click the settings cog in the bottom left of the window and click on the runtime tab. Here make sure an appropriate llama.cpp variant is installed. Vulkan works but GPU specific should be faster. Set the preferred variant at the top.

In the upper right click "+ Load Model" and select your chosen model with "manually choose model load parameters" enabled.
<img width="767" height="1143" alt="lmstudio_loadsettings" src="https://github.com/user-attachments/assets/242ee2bd-8d60-4eed-b516-dbc4810e758a" />

Tweak settings until the estimated usage is around or below your VRAM amount. Increase your physical batch size to improve tokens/second speed, set your KV cache quantization type to at most Q8, if you must have longer context lower will be more ram efficient but with either a tradeoff of capability or speed depending on if you use Q4_0 or the others.

Right side:
<img width="411" height="1109" alt="lmstudio_inference" src="https://github.com/user-attachments/assets/e569c3d8-0285-4b1f-a241-9dfbd1384ca9" />

Create a preset and generally follow these settings. A temperature of 0.6 is recommended for Qwen models for coding workflows.


#VSCode and Zoo Code setup:
Open VSCode, then install the Zoo Code extension from the marketplace. Once installed open it and follow the onboarding. Choose LM Studio as your provider, base URL will be http://127.0.0.1:1234. Model will be whatever you downloaded.

Context:
Scroll to the bottom and change Condensing Trigger Threshold to something like 90%

Optional:
Notifications: Enable sound effects or text to speech.
UI: Require Ctrl+Enter to send for formatting.

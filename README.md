<h2 align="center">
  🌦️ WeatherEdit: Controllable Weather Editing with 4D Gaussian Field
</h2>
<p align="center">
  <em>
    <a href="https://scholar.google.com/citations?user=EFV2q-4AAAAJ&hl=en">Chenghao Qian</a><sup>1</sup>,
    <a href="https://scholar.google.com/citations?user=uBjSytAAAAAJ&hl=en">Wenjing Li</a><sup>1</sup>,
    <a href="https://example.com/guo">Yuhu Guo</a><sup>2</sup>,
    <a href="https://scholar.google.com/citations?user=55G7VxoAAAAJ&hl=en">Gustav Markkula</a><sup>1</sup>
  </em>
</p>


<p align="center">
  <img src="https://github.com/user-attachments/assets/4483e88e-4552-4245-9c29-2c84abf78788" alt="image" width="390">
</p>



<p align="center">
  <a href="https://arxiv.org/abs/1234.56789">
    <img src="https://img.shields.io/badge/arXiv-article-red" alt="arXiv">
  </a>
  <a href="https://example.com">
    <img src="https://img.shields.io/badge/Project-link-blue" alt="Project">
  </a>
</p>




<p align="center">
  A <strong>Realistic, Controllable</strong> and <strong>Scalable</strong> Framework for Weather Editing.
</p><div align="center">
  <img src="https://github.com/user-attachments/assets/e5b74e41-d25f-4a25-af42-99a95bcdb04e" width="800"/>
</div>

<div align="center">
  <img src="https://github.com/user-attachments/assets/b2b46e74-010d-4d80-a153-e4561668cc0f" width="800"/>
</div>

---

- 🎨 Flexible control over **weather types** (e.g., rain, fog, snow)
- 🌡️ Precise **weather severity** adjustment (light, moderate, heavy)
- 🖼️ **Multi-view and multi-frame consistency** for driving and general scenes

---

## 💡 Quick Start

### A. General Weather Editing (`General Scene/`)
A lightweight version for easy integration into general scenes, without requiring local field alignment.
We provide step-by-step instructions for rendering weather particles from scratch, as well as guidance for modular integration into your own pipeline.

#### 🔶 (a) Rendering Weather Particle from Scratch  
#### Step 1. Train a Scene with 3D Gaussian Splatting

Prepare your dataset and train (Or you may use our pretrained checkpoint).:

```bash
python train.py -s path/to/data/
```

#### Step 2. Render with Weather Particle

Render with custom weather using:

```bash
python render.py -m path/to/model --weather snow
```


#### 🔶 (b) Plug-and-Play Weather Particle Module (in your 3DGS code)

You can inject weather effects into any Gaussian rendering pipeline.

#### Step 1. Set Weather Configuration

Edit `weather_config.json` to define attributes of particles:

```python
{
  "weather_type": "snow",
  "density": 1500,
  "velocity": [0.0, -0.4, 0.0],
  "scale": 0.04,
  ...
  ...
}
```

#### Step 2. Load Particle Gaussians

In your render script:

```python
from utils.weather_utils import load_particle_config

particle = load_particle_config(gaussians, weather_type=weather_type,config_path="weather_config.json")
particle_gaussians = particle.get_static_gaussians()
```

#### Step 3. Merge Weather with Scene Gaussians (`gaussian_renderer/__init__.py`) 

```python
means3D   = torch.cat((means3D, pg['positions']), dim=0)
means2D   = torch.cat((means2D, torch.zeros_like(pg['positions'])), dim=0)
opacity   = torch.cat((opacity, pg['opacity']), dim=0)
scales    = torch.cat((scales, pg['scaling']), dim=0)
rotations = torch.cat((rotations, pg['rotation']), dim=0)
```

#### Step 4. Render and Update Weather Particles

```python
rendering = render(interp_cam, gaussians, pipeline, background, pg=particle_gaussians)["render"]
if particle:
    particle.update_positions(delta_time)
```

This allows for **dynamic simulation** of weather effects that evolve over time and viewpoint.

---

### B. Driving Scene Editing (`Driving Scene/`) 🚘

Stay Tuned.

---

## 📌 Citation

If you use WeatherEdit in your work, please cite:

```bibtex
@article{qian2025weatheredit,
  title={WeatherEdit: Controllable Weather Editing with 4D Gaussian Field},
  author={Qian, Chenghao and Guo, Yuhu and Li, Wenjing and Markkula, Gustav},
  journal={ICCV},
  year={2025}
}
```

---

## 📬 Contact

For questions, suggestions, or collaborations:

- 📧 tscq@leeds.ac.uk
- 🌐 [Project Page](https://your-project-page-link.com)

---

Thanks for your interest in WeatherEdit! We hope it helps bring new life to your 3D scenes 🌧️🌨️🌫️

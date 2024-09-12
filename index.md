---
layout: default
logo: video-llamb.png
title: > 
  VideoLLaMB: Long-Context Video Understanding with Recurrent Memory Bridges
authors:
    - name: Yuxuan Wang
      tag: 1
      url: https://patrick-tssn.github.io/
    - name: Cihang Xie
      url: https://cihangxie.github.io/
      tag: 2
    - name: Yang Liu
      url: http://www.csyangliu.com/
      tag: 3
    - name: Zilong Zheng
      url: https://zilongzheng.github.io
      tag: 1, <i class="fa fa-envelope"></i>
affiliations:
    - name: BIGAI
      tag: 1
    - name: UCSC
      tag: 2
    - name: PKU
      tag: 3
# misc: > 
  # <sup><i class="fa fa-envelope"></i></sup> Corresponding authors.

arxiv: https://arxiv.org/abs/2409.01071
code: https://github.com/bigai-nlco/VideoLLaMB
links:
  - name: VideoLLaMB-7B
    icon: "&#129303;"
    url: https://huggingface.co/ColorfulAI/VideoLLaMB
  - name: MM-NIAVH
    icon: <i class="fab fa-github"></i>
    url: https://github.com/bigai-nlco/NeedleInAVideoHaystack
  - name: Demo (Coming Soon)
    icon: <i class="fas fa-globe"></i>
---


<section class="hero teaser">
  <div class="container is-max-desktop">
    <div class="hero-body">
<div class="columns is-centered has-text-centered">

<div class="column">
<figure class="image">
      <figcaption><span class="dnerf">VideoLLaMB</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</figcaption>
      <img src="{{ '/assets/img/videollamb_niavh.png' | relative_url }}" />
</figure>


<figure class="image">
      <figcaption><span class="dnerf">VideoLLaMB w/o Memory Retrieval</span></figcaption>
      <img src="{{ '/assets/img/videollamb_niavh_nor.png' | relative_url }}" />
</figure>

</div>

<div class="column">
<figure class="image">
      <figcaption><span class="dnerf">LongVA (<a href="https://github.com/EvolvingLMMs-Lab/LongVA">Zhang et. al., 2024</a>)</span></figcaption>
      <img src="{{ '/assets/img/longva.png' | relative_url }}" />
</figure>

<figure class="image">
      <figcaption><span class="dnerf">MA-LLM (<a href="https://github.com/boheumd/MA-LMM">He et. al., 2024</a>)</span></figcaption>
      <img src="{{ '/assets/img/mallm.png' | relative_url }}" />
</figure>

</div>


</div>


<details closed>
<summary><b>More comparisons</b></summary>
<div class="columns is-centered has-text-centered">

<div class="column">
<figure class="image">
      <figcaption><span class="dnerf"><a href="https://huggingface.co/ermu2001/pllava-7b">PLLaVA-7B</a>  (Run 6/5/2024)</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</figcaption>
      <img src="{{ '/assets/img/pllava.png' | relative_url }}" />
</figure>

</div>

<div class="column">
<figure class="image">
      <figcaption><span class="dnerf"><a href="https://huggingface.co/lmms-lab/LLaVA-NeXT-Video-7B-DPO">LLaVA-NeXT-Video-DPO-7B</a> (Run 6/5/2024) </span></figcaption>
      <img src="{{ '/assets/img/llavanext.png' | relative_url }}" />
</figure>

</div>


</div>
</details>

<figcaption style="padding-top:10px;"><span class="dnerf">Figure 1.</span> <b>Comparison of long video understanding models on <a href="#stress-test-needle-in-a-video-haystack">Needle In a Video Haystack (NIAVH)</a>.</b> We set the context length to 320 seconds due to existing models' ability and set the frame rate to 1 fps to ensure the input contains the needle. The X-axis indicates the video length, and the Y-axis is the depth of the insertion point.</figcaption>

    </div>
  </div>
</section>



<section class="section">
    <div class="container is-max-desktop" markdown="1">

## Abstract
{:.title .has-text-centered}


VideoLLaMB is a novel long video comprehension framework utilizing Memory Bridge Layers with recurrent memory tokens to encode 100% video content without discarding critical visual cues.

✨ Highlights:

1. **Comprehensive long video understanding.** VideoLLaMB-7B reached the state-of-the-art performance among 7B models trained on vicuna-7b and videochat2 video on [EgoSchema](https://egoschema.github.io/), [NexTQA](https://github.com/doc-doc/NExT-QA) and [MVBench](https://github.com/OpenGVLab/Ask-Anything/blob/main/video_chat2/MVBENCH.md), reaching *8x longer video length* with robust performance in comparison to [PLLaVA](https://pllava.github.io/). 

2. **Memory-based egocentric planning.** VideoLLaMB achieves the best performance among all video-language models on [EgoPlan](https://github.com/ChenYi99/EgoPlan), with an improvement of $$2.06$$ over PLLaVA.

3. **Training-free streaming captioning.** With our [SceneTiling algorithm](#scene-tiling-segmentation-with-semantics), VideoLLaMB can capture the dynamics with in a streaming video and directly predict the streaming captions in real-time, without the need to process the entire video sequence beforehand.

4. **Enhanced frame retrieval on needle in a video haystack (NIAVH).** We present the "Needle in a Video Haystack" (NIAVH) benchmark to evaluate long video understanding over needle of different modalities comprehensively ([details 👉](#stress-test-needle-in-a-video-haystack)). In the pressure test ranging from 1 to 300 seconds in length, VideoLLaMB consistently retrieves the correct image needles at various depths, outperforming other methods as video length increases.
        
</div>

<div class="columns is-centered has-text-centered">

<div class="column is-four-fifths">
<figure class="image">
      <img src="{{ '/assets/img/framework_new.png' | relative_url }}" />
      <figcaption><span class="dnerf">Figure 2.</span> <b>An overview of VideoLLaMB.</b> <a href="#technical-details">Technical details.</a> </figcaption>
</figure>

</div>

</div>


</section>





<section class="section"   style="background-color:#efeff081" >
    <div class="container" markdown="1">

## Long-form Video Understanding
{:.title.is-3.has-text-centered}


<div class="table-container has-text-centered" markdown="1" style="font-size:15px" >

| **Method** | **Vision Encoder** | **LLM Size** | **AS** | **AP** | **AA** | **FA** | **UA** | **OE** | **OI** | **OS** | **MD** | **AL** | **ST** | **AC** | **MC** | **MA** | **SC** | **FP** | **CO** | **EN** | **ER** | **CI** | **Avg.** |
|:------------|:--------------------:|:--------------:|--------|--------|--------|--------|--------|--------|--------|--------|--------|--------|--------|--------|--------|--------|--------|--------|--------|--------|--------|--------|--------|
| GPT-4V | GPT-4V | / | 55.5 | 63.5 | 72.0 | 46.5 | 73.5 | 18.5 | 59.0 | 29.5 | 12.0 | 40.5 | 83.5 | 39.0 | 12.0 | 22.5 | 45.0 | 47.5 | 52.0 | 31.0 | 59.0 | 11.0 | 43.5 |
| *Image MLLMs*              |
| mPLUG-Owl-I | ViT-L | 7B | 25.0 | 20.0 | 44.5 | 27.0 | 23.5 | 36.0 | 24.0 | 34.0 | 23.0 | 24.0 | 34.5 | 34.5 | 22.0 | 31.5 | 40.0 | 24.0 | 37.0 | 25.5 | 21.0 | 37.0 | 29.4 |
| LLaMA-Adapter | ViT-B | 7B | 23.0 | 28.0 | 51.0 | 30.0 | 33.0 | 53.5 | 32.5 | 33.5 | 25.5 | 21.5 | 30.5 | 29.0 | 22.5 | 41.5 | 39.5 | 25.0 | 31.5 | 22.5 | 28.0 | 32.0 | 31.7 |
| BLIP2| ViT-G | 2.7B | 24.5 | 29.0 | 33.5 | 17.0 | 42.0 | 51.5 | 26.0 | 31.0 | 25.5 | 26.0 | 32.5 | 25.5 | 30.0 | 40.0 | 42.0 | 27.0 | 30.0 | 26.0 | 37.0 | 31.0 | 31.4 |
| Otter-I| ViT-L | 7B | 34.5 | 32.0 | 39.5 | 30.5 | 38.5 | 48.5 | 44.0 | 29.5 | 19.0 | 25.5 | 55.0 | 20.0 | 32.5 | 28.5 | 39.0 | 28.0 | 27.0 | 32.0 | 29.0 | 36.5 | 33.5 |
| MiniGPT-4| ViT-G | 7B | 16.0 | 18.0 | 26.0 | 21.5 | 16.0 | 29.5 | 25.5 | 13.0 | 11.5 | 12.0 | 9.5 | 32.5 | 15.5 | 8.0 | 34.0 | 26.0 | 29.5 | 19.0 | 9.9 | 3.0 | 18.8 |
| InstructBLIP| ViT-G | 7B | 20.0 | 16.5 | 46.0 | 24.5 | 46.0 | 51.0 | 26.0 | 37.5 | 22.0 | 23.0 | 46.5 | <span class="is-1">42.5</span> | 26.5 | 40.5 | 32.0 | 25.5 | 30.0 | 25.5 | 30.5 | 38.0 | 32.5 |
| LLaVA| ViT-L | 7B | 28.0 | 39.5 | 63.0 | 30.5 | 39.0 | 53.0 | 41.0 | 41.5 | 23.0 | 20.5 | 45.0 | 34.0 | 20.5 | 38.5 | 47.0 | 25.0 | 36.0 | 27.0 | 26.5 | 42.0 | 36.0 |
| *Video MLLMs* |
| Video-LLaMA| CLIP-G | 7B | 27.5 | 25.5 | 51.0 | 29.0 | 39.0 | 48.0 | 40.5 | 38.0 | 22.5 | 22.5 | 43.0 | 34.0 | 22.5 | 32.5 | <span class="is-3">45.5</span> | 32.5 | 40.0 | 30.0 | 21.0 | 37.0 | 34.1 |
| LLaMA-Adapter| ViT-B | 7B | 23.0 | 28.0 | 51.0 | 30.0 | 33.0 | 53.5 | 32.5 | 33.5 | 25.5 | 21.5 | 30.5 | 29.0 | 22.5 | 41.5 | 39.5 | 25.0 | 31.5 | 22.5 | 28.0 | 32.0 | 31.7 |
| Video-ChatGPT| ViT-L | 7B | 23.5 | 26.0 | 62.0 | 22.5 | 26.5 | 54.0 | 28.0 | <span class="is-2">40.0</span> | 23.0 | 20.0 | 31.0 | 30.5 | 25.5 | 39.5 | <span class="is-1">48.5</span> | 29.0 | 33.0 | 29.5 | 26.0 | 35.5 | 32.7 |
| VideoChat| CLIP-G | 7B | 33.5 | 26.5 | 56.0 | 33.5 | 40.5 | 53.0 | 40.5 | 30.0 | 25.5 | 27.0 | 48.5 | 35.0 | 20.5 | 42.5 | <span class="is-2">46.0</span> | 26.5 | 41.0 | 23.5 | 23.5 | 36.0 | 35.5 |
| VideoChat2$$^\beta$$| UMT-L | 7B | <span class="is-1">66.0</span> | 47.5 | <span class="is-3">83.5</span> | <span class="is-1">49.5</span> | <span class="is-2">60.0</span> | 58.0 | <span class="is-1">71.5</span> | <span class="is-1">42.5</span> | 23.0 | 23.0 | <span class="is-1">88.5</span> | 39.0 | 42.0 | 58.5 | 44.0 | <span class="is-1">49.0</span> | 36.5 | <span class="is-1">35.0</span> | 40.5 | <span class="is-1">65.5</span> | <span class="is-2">51.1</span> |
| PLLaVA 7B$$^\alpha$$| ViT-L | 7B | <span class="is-2">58.0</span> | <span class="is-2">49.0</span> | 55.5 | 41.0 | <span class="is-1">61.0</span> | 56.0 | <span class="is-2">61.0</span> | 36.0 | 23.5 | 26.0 | 82.0 | 39.5 | 42.0 | 52.0 | 45.0 | <span class="is-2">42.0</span> | <span class="is-1">53.5</span> | <span class="is-3">30.5</span> | <span class="is-1">48.0</span> | 31.0 | 46.6 |
| <span class="is-ignorable">PLLaVA 13B</span>$$^\alpha$$| ViT-L| 13B| 66.0| 53.0| 65.5| 45.0| 65.0| 58.0| 64.5| 35.5| 23.5| 30.0| 85.0| 39.5| 45.5| 57.0| 47.5| 49.5| 49.0| 33.0| 53.0| 37.0| <span class="is-ignorable">50.1</span> |
| **VideoLLaMB**$$^\alpha$$ | ViT-L | 7B | 52.0 | <span class="is-1">50.5</span> | <span class="is-2">85.5</span> | 42.5 | 51.0 | 69.5 | 56.0 | <span class="is-3">38.5</span> | 41.0 | 24.0 | 69.5 | <span class="is-3">40.0</span> | <span class="is-2">48.0</span> | <span class="is-2">71.5</span> | 43.5 | 34.5 | 41.5 | 29.5 | 38.0 | <span class="is-2">60.0</span> | <span class="is-3">49.3</span> |
| **VideoLLaMB**$$^\beta$$ | ViT-L | 7B | <span class="is-3">54.5</span> | 47.0 | <span class="is-1">86.5</span> | <span class="is-2">44.5</span> | <span class="is-3">52.0</span> | <span class="is-1">79.0</span> | <span class="is-3">58.5</span> | 32.0 | <span class="is-1">47.0</span> | <span class="is-1">33.0</span> | <span class="is-2">82.5</span> | <span class="is-2">40.5</span> | <span class="is-1">52.0</span> | <span class="is-1">82.0</span> | 40.5 | <span class="is-3">37.5</span> | <span class="is-2">43.0</span> | <span class="is-2">31.0</span> | <span class="is-2">42.5</span> | <span class="is-2">60.0</span> | <span class="is-1">52.5</span> |
{: .table.is-fullwidth.is-narrow.is-hoverable }
<span class="dnerf">Table 1.</span> <b>Results on <a href="https://github.com/OpenGVLab/Ask-Anything/blob/main/video_chat2/MVBENCH.md">MVBench</a> multi-choice question answering.</b> The top 3 results among 7B models are highlighted. $$\alpha$$: training with data from <a href="https://pllava.github.io">PLLaVA</a>. $$\beta$$: training with data from <a href="https://github.com/OpenGVLab/Ask-Anything/blob/main/video_chat2/MVBENCH.md">MVBench</a>.


</div>

<div class="columns is-centered has-text-centered">
<div class="column is-centered "  markdown="1">

<table class=" table is-narrow is-hoverable is-fullwidth">
  <thead>
    <tr>
      <th><b>Model</b></th>
      <th><b>LLM</b></th>
      <th><b>Frames</b></th>
      <th><b>Accuracy</b></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align: left">GPT4-o</td>
      <td>OpenAI API</td>
      <td>16</td>
      <td>72.2</td>
    </tr>
    <tr>
      <td colspan="3" style="text-align: left"><small><i>Retrieval-based Video-Language Models</i></small></td>
    </tr>
    <tr>
      <td style="text-align: left">LongViViT*</td>
      <td>-</td>
      <td>256</td>
      <td>56.8</td>
    </tr>
    <tr>
      <td style="text-align: left">MC-ViT-L*</td>
      <td>-</td>
      <td>128</td>
      <td>62.5</td>
    </tr>
    <tr>
      <td colspan="3" style="text-align: left"><small><i>Generative Video-Language Models</i></small></td>
    </tr>
    <tr>
      <td style="text-align: left">SeViLA</td>
      <td>Flan-T5-XL</td>
      <td>32</td>
      <td>25.8</td>
    </tr>
    <tr>
      <td style="text-align: left">mPLUG-Owl</td>
      <td>LLaMA-7B</td>
      <td>5</td>
      <td>33.8</td>
    </tr>
    <tr>
      <td style="text-align: left">VideoLLaVA</td>
      <td>Vicuna-7B</td>
      <td>8</td>
      <td>40.2</td>
    </tr>
    <tr>
      <td style="text-align: left">LLaVA-NeXT-Video-DPO</td>
      <td>Vicuna-7B</td>
      <td>32</td>
      <td>41.6</td>
    </tr>
    <tr>
      <td style="text-align: left">PLLaVA</td>
      <td>Vicuna-7B</td>
      <td>16 (16)</td>
      <td>45.6</td>
    </tr>
    <tr>
      <td style="text-align: left">PLLaVA</td>
      <td>Vicuna-7B</td>
      <td>32 (16)</td>
      <td>43.8</td>
    </tr>
    <tr>
      <td style="text-align: left"><b>VideoLLaMB</b></td>
      <td>Vicuna-7B</td>
      <td>32 (8)</td>
      <td><b>53.8</b></td>
    </tr>
  </tbody>
</table>

<span class="dnerf">Table 2.</span> <b>Results on subset of [EgoSchema](https://egoschema.github.io/) under zero-shot setting.</b>  $$^*$$ indicates that the model has been fine-tuned using the training data from [EgoSchema](https://egoschema.github.io/).


</div>

<div class="column  is-centered" markdown="1">

<table class=" table is-narrow is-hoverable is-fullwidth">
    <thead>
        <tr>
            <th><b>Model</b></th>
            <th><b>Temporal</b></th>
            <th><b>Causal</b></th>
            <th><b>Description</b></th>
            <th><b>All</b></th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td  style="text-align: left">GPT4-o</td>
            <td>70.3</td>
            <td>78.0</td>
            <td>80.8</td>
            <td>76.0</td>
        </tr>
        <tr>
            <td colspan="5" style="text-align: left"><small><i>Retrieval-based Video-Language Models</i></small></td>
        </tr>
        <tr>
            <td  style="text-align: left">AIO*</td>
            <td>48.0</td>
            <td>48.6</td>
            <td>63.2</td>
            <td>50.6</td>
        </tr>
        <tr>
            <td style="text-align: left">VQA-T*</td>
            <td>49.6</td>
            <td>51.5</td>
            <td>63.2</td>
            <td>52.3</td>
        </tr>
        <tr>
            <td style="text-align: left">ATP*</td>
            <td>50.2</td>
            <td>53.1</td>
            <td>66.8</td>
            <td>54.3</td>
        </tr>
        <tr>
            <td style="text-align: left">VGT*</td>
            <td>52.3</td>
            <td>55.1</td>
            <td>64.1</td>
            <td>55.0</td>
        </tr>
        <tr>
            <td style="text-align: left">MIST-CLIP*</td>
            <td>56.6</td>
            <td>54.6</td>
            <td>66.9</td>
            <td>57.1</td>
        </tr>
        <tr>
            <td colspan="5"  style="text-align: left"><small><i>Generative Video-Language Models</i></small></td>
        </tr>
        <tr>
            <td style="text-align: left">SeViLA</td>
            <td>61.5</td>
            <td>61.3</td>
            <td>75.6</td>
            <td>63.6</td>
        </tr>
        <tr>
            <td style="text-align: left">LLaMA-VID</td>
            <td>53.8</td>
            <td>60.0</td>
            <td>73.0</td>
            <td>59.5</td>
        </tr>
        <tr>
            <td style="text-align: left">VideoLLaVA</td>
            <td>56.9</td>
            <td>61.0</td>
            <td>75.0</td>
            <td>61.3</td>
        </tr>
        <tr>
            <td style="text-align: left">LLaVA-NeXT-Video-DPO</td>
            <td>55.6</td>
            <td>61.0</td>
            <td>73.9</td>
            <td>61.3</td>
        </tr>
        <tr>
            <td style="text-align: left">PLLaVA*</td>
            <td>62.2</td>
            <td>68.5</td>
            <td><strong>79.7</strong></td>
            <td>68.2</td>
        </tr>
        <tr>
            <td style="text-align: left"><strong>VideoLLaMB*</strong></td>
            <td><strong>66.8</strong></td>
            <td><strong>71.6</strong></td>
            <td>78.4</td>
            <td><strong>71.1</strong></td>
        </tr>
    </tbody>
</table>

<span class="dnerf">Table 3.</span> <b>Comparison accuracy on [NExT-QA](hhttps://github.com/doc-doc/NExT-QA).</b>  $$^*$$ indicates that the instruction data includes the training data from NExT-QA.

</div>


</div>


<details open>
<summary><b>Streaming Caption</b></summary>
<div class="is-centered has-text-centered" style="background-color:rgba(117, 209, 215, 0.1)" >

<h4 style="font-size: 20px; padding: 10px 0 10px;">Task: Describe the streaming video in real-time.</h4>
  <video poster id="streaming_caption" autoplay controls muted loop playsinline height="70%">
    <source src="{{ 'streaming_caption.mp4' | prepend: '/assets/img/' | relative_url }}" />
  </video>
</div>
</details>

<!-- </div>
</section>


<section class="section" >
    <div class="container" markdown="1"> -->

<br>

## Egocentric Embodied Planning
{:.title.is-3.has-text-centered}


<div class="columns is-centered has-text-centered">
<div class="column is-centered "  markdown="1">

<table class="table is-narrow is-hoverable is-fullwidth">
  <thead>
    <tr>
      <th style="text-align: left; font-weight: bold; text-align: center;">Model</th>
      <th style="font-weight: bold; text-align: center;">LLM</th>
      <th style="font-weight: bold; text-align: center;">Accuracy</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align: left;">GPT-4V</td>
      <td style="text-align: center;">OpenAI API</td>
      <td style="text-align: center;">37.98</td>
    </tr>
    <tr>
      <td colspan="3" style="text-align: left; font-size: small; font-style: italic;">Image-Language Model</td>
    </tr>
    <tr>
      <td style="text-align: left;">Qwen-VL-Chat</td>
      <td style="text-align: center;">Qwen-7B</td>
      <td style="text-align: center;">26.32</td>
    </tr>
    <tr>
      <td style="text-align: left;">LLaVA-1.5</td>
      <td style="text-align: center;">Vicuna-7B</td>
      <td style="text-align: center;">26.80</td>
    </tr>
    <tr>
      <td style="text-align: left;">SEED-LLaMA</td>
      <td style="text-align: center;">LLaMA2-Chat-13B</td>
      <td style="text-align: center;">29.93</td>
    </tr>
    <tr>
      <td style="text-align: left;">InternLM-Xcomposer</td>
      <td style="text-align: center;">InternLM-7B</td>
      <td style="text-align: center;">34.4</td>
    </tr>
    <tr>
      <td colspan="3" style="text-align: left; font-size: small; font-style: italic;">Video-Language Model</td>
    </tr>
    <tr>
      <td style="text-align: left;">VideoChatGPT</td>
      <td style="text-align: center;">LLaMA-7B</td>
      <td style="text-align: center;">26.35</td>
    </tr>
    <tr>
      <td style="text-align: left;">Valley</td>
      <td style="text-align: center;">LLaMA-13B</td>
      <td style="text-align: center;">26.17</td>
    </tr>
    <tr>
      <td style="text-align: left;">VideoLLaMA</td>
      <td style="text-align: center;">LLaMA2-Chat-7B</td>
      <td style="text-align: center;">29.85</td>
    </tr>
    <tr>
      <td style="text-align: left;">LLaVA-NeXT-Video</td>
      <td style="text-align: center;">Vicuna-7B</td>
      <td style="text-align: center;">28.96</td>
    </tr>
    <tr>
      <td style="text-align: left;">PLLaVA</td>
      <td style="text-align: center;">Vicuna-7B</td>
      <td style="text-align: center;">30.26</td>
    </tr>
    <tr>
      <td style="text-align: left; font-weight: bold;">VideoLLaMB-7B</td>
      <td style="text-align: center;">Vicuna-7B</td>
      <td style="text-align: center; font-weight: bold;">32.32</td>
    </tr>
  </tbody>
</table>

<!-- <figcaption> -->
<span class="dnerf">Table 4.</span> <b>Results on [EgoPlan](https://github.com/ChenYi99/EgoPlan) under Zero-shot setting.</b>


</div>

<div class="column  is-centered has-text-centered" markdown="1">

<figure class="image">
      <b>Goal:</b> <em>clean and organize kitchen</em>
      <img src="{{ '/assets/img/case.png' | relative_url }}" />
      <figcaption><span class="dnerf">Figure 2.</span> <b>Qualitative results on <a href="https://github.com/ChenYi99/EgoPlan">EgoPlan</a>.</b> </figcaption>
</figure>


</div>


</div>


</div>
</section>

<section class="section">
    <div class="container is-max-desktop" markdown="1">

## Stress Test: "Needle In A Video Haystack"
{:.title.is-3.has-text-centered}

<div class="columns is-centered has-text-centered">
<div class="column">

<figure class="image">
      <b>Needle:</b> "<em>A young man is sitting on a piece of cloud in the sky, reading a book.</em>"
      <img src="{{ '/assets/img/case_needle.png' | relative_url }}" />
      <figcaption><span class="dnerf">Figure 3.</span> <b>An example of NIAVH.</b> </figcaption>
</figure>

</div>
</div>

We utilize ego-centric videos from the Ego4D dataset as the "haystack". Within this haystack, we seek to locate the "needle", which we provide in three distinct modalities. For the textual modality, we supply a crafted description. For the image modality, we employ DALL-E to create an image that visually represents this description. For the video modality, we use Sora to generate a short video clip based on the same description. In each case, the "needle" - whether text, image, or video - is set to a duration of 1 second.


</div>
</section>

<section class="section"   style="background-color:#efeff081" >
    <div class="container is-max-desktop" markdown="1">


## Technical Details
{:.title.is-3.has-text-centered}


### Scene Tiling: Segmentation with Semantics
{:.title.is-4}

We introduce SceneTilling, a *model-free* scene segmentation algorithm, to divide the entire video sequence into video segments such that each segment is semantically non-overlap with others, *i.e.*, inter-segment coherence. Formally, given a sequence of $$n$$ frames $$\{v_1, v_2, \ldots, v_n\}$$, the SceneTiling algorithm is as follows.
1. Compute the cosine similarity $$S_C(\cdot, \cdot)$$ between adjacent frame pairs using the [CLS] token from ViT, resulting in a sequence of similarity scores $$\{c_1, c_2, \ldots, c_{n-1}\}$$, where $$c_i = S_C ({\rm ViT}(v_i), {\rm ViT}(v_{i+1}))$$. 
2. Calculate the depth score for each point as $$d_i = \left(cl_i+cr_i-2c_i\right)/{2}$$, where $$cl_i$$ and $$cr_i$$ are the highest score to the left and right of $$c_i$$, respectively. A higher depth score indicates that the surrounding similarity is greater than at the point itself.
3. Calculate the expectation $$\mu$$ and variance $$\sigma$$ of the depth scores $$\{d_1, d_2, \ldots, d_{n-1}\}$$. Set the segmentation threshold as $$\mu + \alpha \cdot \sigma$$, where $$\alpha$$ is a hyperparameter controlling the likelihood of segmenting the video.
4. Select the $$K-1$$ depth scores that exceed the threshold to divide the video into $$K$$ semantic segments $$\{s_1, s_2, \ldots, s_K\}$$. Each segment represents a relatively independent semantic unit consisting of a sequence of frames.
<br>

### Recurrent Memory Bridge Layers
{:.title.is-4}


We devised a novel Recurrent Memory Bridge Layer, implemented as a multi-layer Transformer block, that integrates recurrent memory tokens within bridge layers to enhance the linear layer's memorization ability. 

For each video segment $$s_i$$, we prepend a fixed number of memory tokens, denoted as $$[m_i; s_i]$$, where $$m_i$$ represents the memory tokens. Subsequently, we apply standard self-attention to this sequence, yielding $$[m_{i+1}; o_{i}] = {\rm BridgeLayer}([m_i; s_i])$$. Here, $$m_{i+1}$$ is the updated memory token, and $$o_{i}$$ is the visual representation from the bridge layers. 



As such, the Memory Bridge can **compress past video into memory tokens while preserving current video scenes through projection without losing detailed information by compressing**.

<!-- Formally, for each video segment $$s_i$$, we prepend a fixed number of memory tokens, denoted as $$[m_i; s_i]$$, where $$m_i$$ represents the memory tokens. Subsequently, we apply standard self-attention to this sequence, yielding $$[m_{i+1}; o_{i+1}] = {\rm BridgeLayer}([m_i; s_i])$$. Here, $$m_{i+1}$$ is the updated memory token, and $$o_{i+1}$$ is the refreshed visual representation.  -->
<!-- This process is carried out recursively, traversing the semantic video segments while updating the memory tokens. After a total of $$k$$ steps, the final output $$o_k$$ can be obtained as the condensed visual representation of the video sequence. This methodology allows us to encode an entire video into a concise sequence while retaining all pertinent information from previous segments. -->


<br>

### Memory Cache with Retrieval
{:.title.is-4}

One of the primary challenges associated with recurrent memory bridge layers is the potential for gradient vanishing, which can impede the model's ability to learn long-range dependencies. To mitigate this issue, we propose the incorporation of a **memory cache** with a retrieval strategy designed to preserve previous states of memory.

**Memory Attention** At each timestep $$i$$, the system stores all previous memory tokens in a memory cache, denoted as $$M_i = [m_1, \ldots, m_i]$$. We employ a self-retrieval mechanism to update the current memory token $$m_i$$. Specifically, we treat $$m_i$$ as a query and the concatenated memory cache $$M_i$$ as key and value. The model performs a standard multi-head cross-attention operation to integrate information from previous timesteps into the current memory state, yielding the updated memory token

$$m_{i+1} = \text{Softmax}\left(\frac{W_i^Q m_i (W_i^K M_i)^\top}{\sqrt{d_k}}\right) W_i^V M_i,$$

where $$W_i^Q, W_i^K, W_i^V$$ are weight martices for query, key and value, respectively.

</div>
</section>




<section class="section">
    <div class="container is-max-desktop" markdown="1">
    
## Citation
{:.title}

```bibtex
@article{wang2024videollamb,
    title={VideoLLaMB: Long-context Video Understanding with Recurrent Memory Bridges},
    author={Wang, Yuxuan and Xie, Cihang and Liu, Yang and Zheng, Zilong},
    journal={arXiv preprint arXiv:2409.01071},
    year={2024}
}
```

</div>
</section>

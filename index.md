---
layout: default
logo: video-llamb.png
title: >
  Video-XL: Extra-Long Vision Language Model for Hour-Scale Video Understanding 
authors:
    - name: Yan Shu
      tag: 1, 2
      url:
    - name: Peitian Zhang
      url: 
      tag: 3
    - name: Zheng Liu
      url: 
      tag: 2† <!--, <i class="fa fa-envelope"></i>-->
    - name: Minghao Qin 
      url: 
      tag: 2, 4 
    - name: Junjie Zhou 
      tag: 2, 5
      url:
    - name: Tiejun Huang 
      tag: 2, 6
      url:
    - name: Bo Zhao 
      tag: 1, 2†
      url:   
affiliations:
    - name: Shanghai Jiaotong University
      tag: 1
    - name: Beijing Academy of Artificial Intelligence
      tag: 2
    - name: Renmin University of China
      tag: 3
    - name: Chinese Academy of Sciences
      tag: 4
    - name: Beijing Universiy of Posts and Telecommunications
      tag: 5
    - name: Peking University
      tag: 6
      
# misc: > 
  # <sup><i class="fa fa-envelope"></i></sup> Corresponding authors.
    

arxiv: https://arxiv.org/pdf/2409.14485
code: https://github.com/VectorSpaceLab/Video-XL
links:
  - name: Video-XL7B 1024
    icon: "&#129303;"
    url: https://huggingface.co/sy1998/Video_XL/tree/main
---



<section class="section">

<div class="container is-max-desktop" >
      <div><img src="{{ '/assets/img/firstImg.jpg' | relative_url }}" /></div>
      <div class="" style="display: flex">
         <div style="text-align: center;     flex: 1;    padding: 0 5% 0 8%;">
              The comparison of video MLLMs in performance and maximum frames. 
          </div>
         <div style="width: 60%;    text-align: center;    padding: 0 5%;">
            Results on the Needle-in-a-haystack evaluation within a single 80GB GPU. The x-axis represents the
            total number of frames in the video haystack. The y-axis shows the position where the needle image
            is located. Gray grids mean “Out of Memory”. Compared to other models, Video-XL can achieve
            nearly 95% accuracy with maximum 2048 frames.
          </div>
      </div>

</div>


</section>

<section class="section">
    <div class="container is-max-desktop" markdown="1">

## Abstract
{:.title .has-text-centered}


Although current Multi-modal Large Language Models (MLLMs) demonstrate promising results in video understanding, processing extremely long videos remains an ongoing challenge. Typically, MLLMs struggle with handling thousands of visual tokens that exceed the maximum context length, and they suffer from the information decay due to token aggregation. Another challenge is the high computational cost stemming from the large number of video tokens. To tackle these issues, we propose Video-XL, an extra-long vision language model designed for efficient hour-scale video understanding. Specifically, we argue that LLMs can be adapted as effective visual condensers and propose Visual Context Latent Summarization which condenses visual contexts into highly compact forms.  Extensive experiments demonstrate that our model achieves promising results on popular long video understanding benchmarks. For example, Video-XL outperforms the current state-of-the-art method on VNBench by nearly 10% in accuracy.  Moreover, Video-XL presents an impressive balance between efficiency and effectiveness, processing 2048 frames on a single 80GB GPU while achieving nearly 95% accuracy in the Needle-in-a-Haystack evaluation.
        
</div>

<div class="columns is-centered has-text-centered">

<div class="column is-four-fifths">
<figure class="image">
      <img src="{{ '/assets/img/framework_new.jpg' | relative_url }}" />
      <figcaption><span class="dnerf">Figure 2.</span> <b>An overview of VideoXL.</b> </figcaption>
</figure>

</div>

</div>


</section>




<section class="section"   style="background-color:#efeff081" >
    <div class="container is-max-desktop" >  
      <h2 class="title is-3 has-text-centered">Long-form Video Understanding</h2>


      <div style="text-align: center; margin-top: 20px;"><img src="{{ '/assets/img/long-form-video.png' | relative_url }}" /></div>
      <div class="has-text-centered ">
          <span class="dnerf">Table 1.</span> The performance on MLVU and VideoMME
      </div>

      <div style="text-align: center; margin-top: 20px;"><img src="{{ '/assets/img/long-form-video2.png' | relative_url }}" /></div>
      <div class="has-text-centered ">
          <span class="dnerf">Table 1.</span> The performance on VNBench and LongVideoBench
      </div>

      <div style="text-align: center; margin-top: 20px;"><img src="{{ '/assets/img/long-form-video3.png' | relative_url }}" /></div>
      <div class="has-text-centered ">
          Qualitative Results of VideoXL
      </div>




  </div>
</section>




<section class="section">
    <div class="container is-max-desktop" markdown="1">
    
## Citation
{:.title}

```bibtex
@article{shu2024video,
   title={Video-XL: Extra-Long Vision Language Model for Hour-Scale Video Understanding},
   author={Shu, Yan and Zhang, Peitian and Liu, Zheng and Qin, Minghao and Zhou, Junjie and Huang, Tiejun and Zhao, Bo},
   journal={arXiv preprint arXiv:2409.14485},
   year={2024}
}
```

</div>
</section>

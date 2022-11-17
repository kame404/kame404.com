---
title: "3Dモデルのテスト"
description: "3Dモデルのテスト"
date: 2022-10-25T12:00:00+09:00
summary: "3Dモデルのテスト"
draft: false
author: "kame404"
tags: [3D,モデリング]
---
## 3Dモデルのテスト

<body>
    <model-viewer  src="../3d-testing/food.glb" ar ar-modes="webxr scene-viewer quick-look" seamless-poster shadow-intensity="1" camera-controls enable-pan></model-viewer>
</body>
<script type="module" src="https://unpkg.com/@google/model-viewer/dist/model-viewer.min.js"></script>
<style type="text/css">
    model-viewer {
      height: 300px;
      width: 100%;
      border-radius: 12px;
      box-shadow: 0 10px 15px -3px rgb(0 0 0 / 0.1), 0 4px 6px -4px rgb(0 0 0 / 0.1);
      border: 1px solid rgba(128, 128, 128, .2);
    }
</style>
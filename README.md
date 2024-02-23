<div class="Box-sc-g0xbh4-0 bJMeLZ js-snippet-clipboard-copy-unpositioned" data-hpc="true"><article class="markdown-body entry-content container-lg" itemprop="text"><h1 tabindex="-1" dir="auto"><a id="user-content-handwritten-text-recognition-with-tensorflow" class="anchor" aria-hidden="true" tabindex="-1" href="#handwritten-text-recognition-with-tensorflow"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">使用 TensorFlow 进行手写文本识别</font></font></h1>
<ul dir="auto">
<li><strong><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">2023/2 更新：</font><font style="vertical-align: inherit;">提供</font></font><a href="https://githubharald.github.io/text_reader.html" rel="nofollow"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">网络演示</font></font></a><font style="vertical-align: inherit;"></font></strong></li>
<li><strong><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">2023/1 更新：请参阅</font></font><a href="https://github.com/githubharald/HTRPipeline"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">HTRPipeline</font></font></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">获取读取整页的包</font></font></strong></li>
<li><strong><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">2021/2 更新：识别行级文本（多个单词）</font></font></strong></li>
<li><strong><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">2021/1 更新：更强大的模型、更快的数据加载器、字束搜索解码器也适用于 Windows</font></font></strong></li>
<li><strong><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">2020 更新：代码与 TF2 兼容</font></font></strong></li>
</ul>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">使用 TensorFlow (TF) 实现的手写文本识别 (HTR) 系统，并在 IAM 离线 HTR 数据集上进行训练。</font><font style="vertical-align: inherit;">该模型将</font></font><strong><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">单个单词或文本行（多个单词）的图像作为输入</font></font></strong><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">并</font></font><strong><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">输出识别的文本</font></font></strong><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">。</font><font style="vertical-align: inherit;">验证集中 3/4 的单词被正确识别，字符错误率约为 10%。</font></font></p>
<p dir="auto"><a target="_blank" rel="noopener noreferrer" href="https://github.com/githubharald/SimpleHTR/blob/master/doc/htr.png"><img src="https://github.com/githubharald/SimpleHTR/raw/master/doc/htr.png" alt="高温" style="max-width: 100%;"></a></p>
<h2 tabindex="-1" dir="auto"><a id="user-content-run-demo" class="anchor" aria-hidden="true" tabindex="-1" href="#run-demo"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">运行演示</font></font></h2>
<ul dir="auto">
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">下载预训练模型之一
</font></font><ul dir="auto">
<li><a href="https://www.dropbox.com/s/mya8hw6jyzqm0a3/word-model.zip?dl=1" rel="nofollow"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">在单词图像上训练的模型</font></font></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">：仅处理每个图像的单个单词，但在 IAM 单词数据集上给出更好的结果</font></font></li>
<li><a href="https://www.dropbox.com/s/7xwkcilho10rthn/line-model.zip?dl=1" rel="nofollow"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">在文本行图像上训练的模型</font></font></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">：可以处理一张图像中的多个单词</font></font></li>
</ul>
</li>
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">将下载的 zip 文件的内容放入</font></font><code>model</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">存储库的目录中</font></font></li>
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">进入</font></font><code>src</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">目录</font></font></li>
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">运行推理代码：
</font></font><ul dir="auto">
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">执行</font></font><code>python main.py</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">以在单词图像上运行模型</font></font></li>
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">执行</font></font><code>python main.py --img_file ../data/line.png</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">以在文本行的图像上运行模型</font></font></li>
</ul>
</li>
</ul>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">使用文本行模型时的输入图像和预期输出如下所示。</font></font></p>
<p dir="auto"><a target="_blank" rel="noopener noreferrer" href="https://github.com/githubharald/SimpleHTR/blob/master/data/word.png"><img src="https://github.com/githubharald/SimpleHTR/raw/master/data/word.png" alt="测试" style="max-width: 100%;"></a></p>
<div class="snippet-clipboard-content notranslate position-relative overflow-auto"><pre class="notranslate"><code>&gt; python main.py
Init with stored values from ../model/snapshot-13
Recognized: "word"
Probability: 0.9806370139122009
</code></pre><div class="zeroclipboard-container">
    <clipboard-copy aria-label="Copy" class="ClipboardButton btn btn-invisible js-clipboard-copy m-2 p-0 tooltipped-no-delay d-flex flex-justify-center flex-items-center" data-copy-feedback="Copied!" data-tooltip-direction="w" value="> python main.py
Init with stored values from ../model/snapshot-13
Recognized: &quot;word&quot;
Probability: 0.9806370139122009" tabindex="0" role="button">
      <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-copy js-clipboard-copy-icon">
    <path d="M0 6.75C0 5.784.784 5 1.75 5h1.5a.75.75 0 0 1 0 1.5h-1.5a.25.25 0 0 0-.25.25v7.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-1.5a.75.75 0 0 1 1.5 0v1.5A1.75 1.75 0 0 1 9.25 16h-7.5A1.75 1.75 0 0 1 0 14.25Z"></path><path d="M5 1.75C5 .784 5.784 0 6.75 0h7.5C15.216 0 16 .784 16 1.75v7.5A1.75 1.75 0 0 1 14.25 11h-7.5A1.75 1.75 0 0 1 5 9.25Zm1.75-.25a.25.25 0 0 0-.25.25v7.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-7.5a.25.25 0 0 0-.25-.25Z"></path>
</svg>
      <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-check js-clipboard-check-icon color-fg-success d-none">
    <path d="M13.78 4.22a.75.75 0 0 1 0 1.06l-7.25 7.25a.75.75 0 0 1-1.06 0L2.22 9.28a.751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018L6 10.94l6.72-6.72a.75.75 0 0 1 1.06 0Z"></path>
</svg>
    </clipboard-copy>
  </div></div>
<p dir="auto"><a target="_blank" rel="noopener noreferrer" href="https://github.com/githubharald/SimpleHTR/blob/master/data/line.png"><img src="https://github.com/githubharald/SimpleHTR/raw/master/data/line.png" alt="测试" style="max-width: 100%;"></a></p>
<div class="snippet-clipboard-content notranslate position-relative overflow-auto"><pre class="notranslate"><code>&gt; python main.py --img_file ../data/line.png
Init with stored values from ../model/snapshot-13
Recognized: "or work on line level"
Probability: 0.6674373149871826
</code></pre><div class="zeroclipboard-container">
    <clipboard-copy aria-label="Copy" class="ClipboardButton btn btn-invisible js-clipboard-copy m-2 p-0 tooltipped-no-delay d-flex flex-justify-center flex-items-center" data-copy-feedback="Copied!" data-tooltip-direction="w" value="> python main.py --img_file ../data/line.png
Init with stored values from ../model/snapshot-13
Recognized: &quot;or work on line level&quot;
Probability: 0.6674373149871826" tabindex="0" role="button">
      <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-copy js-clipboard-copy-icon">
    <path d="M0 6.75C0 5.784.784 5 1.75 5h1.5a.75.75 0 0 1 0 1.5h-1.5a.25.25 0 0 0-.25.25v7.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-1.5a.75.75 0 0 1 1.5 0v1.5A1.75 1.75 0 0 1 9.25 16h-7.5A1.75 1.75 0 0 1 0 14.25Z"></path><path d="M5 1.75C5 .784 5.784 0 6.75 0h7.5C15.216 0 16 .784 16 1.75v7.5A1.75 1.75 0 0 1 14.25 11h-7.5A1.75 1.75 0 0 1 5 9.25Zm1.75-.25a.25.25 0 0 0-.25.25v7.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-7.5a.25.25 0 0 0-.25-.25Z"></path>
</svg>
      <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-check js-clipboard-check-icon color-fg-success d-none">
    <path d="M13.78 4.22a.75.75 0 0 1 0 1.06l-7.25 7.25a.75.75 0 0 1-1.06 0L2.22 9.28a.751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018L6 10.94l6.72-6.72a.75.75 0 0 1 1.06 0Z"></path>
</svg>
    </clipboard-copy>
  </div></div>
<h2 tabindex="-1" dir="auto"><a id="user-content-command-line-arguments" class="anchor" aria-hidden="true" tabindex="-1" href="#command-line-arguments"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">命令行参数</font></font></h2>
<ul dir="auto">
<li><code>--mode</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">：在“训练”、“验证”和“推断”之间选择。</font><font style="vertical-align: inherit;">默认为“推断”。</font></font></li>
<li><code>--decoder</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">：从 CTC 解码器“bestpath”、“beamsearch”和“wordbeamsearch”中选择。</font><font style="vertical-align: inherit;">默认为“最佳路径”。</font><font style="vertical-align: inherit;">对于选项“wordbeamsearch”，请参阅下面的详细信息。</font></font></li>
<li><code>--batch_size</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">：批量大小。</font></font></li>
<li><code>--data_dir</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">：包含 IAM 数据集的目录（带有子目录</font></font><code>img</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">和</font></font><code>gt</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">）。</font></font></li>
<li><code>--fast</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">：使用LMDB可以更快地加载图像。</font></font></li>
<li><code>--line_mode</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">：训练阅读文本行而不是单个单词。</font></font></li>
<li><code>--img_file</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">：用于推理的图像。</font></font></li>
<li><code>--dump</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">：将 NN 的输出转储到保存在文件夹中的 CSV 文件</font></font><code>dump</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">。</font><font style="vertical-align: inherit;">可用作</font></font><a href="https://github.com/githubharald/CTCDecoder"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">CTCDecoder</font></font></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">的输入。</font></font></li>
</ul>
<h2 tabindex="-1" dir="auto"><a id="user-content-integrate-word-beam-search-decoding" class="anchor" aria-hidden="true" tabindex="-1" href="#integrate-word-beam-search-decoding"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">集成Word Beam搜索解码</font></font></h2>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">可以使用字束搜索解码器来代替 TF 附带的两个解码</font><font style="vertical-align: inherit;">器</font></font><a href="https://repositum.tuwien.ac.at/obvutwoa/download/pdf/2774578" rel="nofollow"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">。</font></font></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">单词被限制为字典中包含的单词，但仍然可以识别任意非单词字符串（数字、标点符号）。</font><font style="vertical-align: inherit;">下图显示了一个示例，其中词束搜索能够识别正确的文本，而其他解码器则失败。</font></font></p>
<p dir="auto"><a target="_blank" rel="noopener noreferrer" href="/githubharald/SimpleHTR/blob/master/doc/decoder_comparison.png"><img src="/githubharald/SimpleHTR/raw/master/doc/decoder_comparison.png" alt="解码器比较" style="max-width: 100%;"></a></p>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">请按照以下说明集成字束搜索解码：</font></font></p>
<ol dir="auto">
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">克隆存储库</font></font><a href="https://github.com/githubharald/CTCWordBeamSearch"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">CTCWordBeamSearch</font></font></a></li>
<li><font style="vertical-align: inherit;"></font><code>pip install .</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">通过在 CTCWordBeamSearch 存储库的根级别</font><font style="vertical-align: inherit;">运行来编译和安装</font></font></li>
<li><font style="vertical-align: inherit;"></font><code>--decoder wordbeamsearch</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">执行时</font><font style="vertical-align: inherit;">指定命令行选项</font></font><code>main.py</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">以实际使用解码器</font></font></li>
</ol>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">该字典是在训练和验证模式下使用 IAM 数据集中包含的所有单词（即还包括验证集中的单词）自动创建的，并保存到文件中</font></font><code>data/corpus.txt</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">。</font><font style="vertical-align: inherit;">此外，可以在文件中找到手动创建的单词字符列表</font></font><code>model/wordCharList.txt</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">。</font><font style="vertical-align: inherit;">波束宽度设置为 50 以符合普通波束搜索解码的波束宽度。</font></font></p>
<h2 tabindex="-1" dir="auto"><a id="user-content-train-model-on-iam-dataset" class="anchor" aria-hidden="true" tabindex="-1" href="#train-model-on-iam-dataset"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">在 IAM 数据集上训练模型</font></font></h2>
<h3 tabindex="-1" dir="auto"><a id="user-content-prepare-dataset" class="anchor" aria-hidden="true" tabindex="-1" href="#prepare-dataset"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">准备数据集</font></font></h3>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">请按照以下说明获取 IAM 数据集：</font></font></p>
<ul dir="auto">
<li><font style="vertical-align: inherit;"><a href="http://www.fki.inf.unibe.ch/databases/iam-handwriting-database" rel="nofollow"><font style="vertical-align: inherit;">在此网站</font></a><font style="vertical-align: inherit;">免费注册</font></font><a href="http://www.fki.inf.unibe.ch/databases/iam-handwriting-database" rel="nofollow"><font style="vertical-align: inherit;"></font></a></li>
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">下载</font></font><code>words/words.tgz</code></li>
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">下载</font></font><code>ascii/words.txt</code></li>
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">在磁盘上为数据集创建一个目录，并创建两个子目录：</font></font><code>img</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">和</font></font><code>gt</code></li>
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">放入</font></font><code>words.txt</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">目录</font></font><code>gt</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">_</font></font></li>
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">将内容（目录</font></font><code>a01</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">, </font></font><code>a02</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">, ...）放入</font><font style="vertical-align: inherit;">目录</font></font><code>words.tgz</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">中</font></font><code>img</code><font style="vertical-align: inherit;"></font></li>
</ul>
<h3 tabindex="-1" dir="auto"><a id="user-content-run-training" class="anchor" aria-hidden="true" tabindex="-1" href="#run-training"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">跑步训练</font></font></h3>
<ul dir="auto">
<li><font style="vertical-align: inherit;"></font><code>model</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">如果您想从头开始训练，</font><font style="vertical-align: inherit;">请从目录中删除文件</font></font></li>
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">进入</font></font><code>src</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">目录并执行</font></font><code>python main.py --mode train --data_dir path/to/IAM</code></li>
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">IAM 数据集分为 95% 训练数据和 5% 验证数据</font></font></li>
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">如果指定了该选项</font></font><code>--line_mode</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">，则模型将在通过将多个单词图像组合成一个而创建的文本行图像上进行训练</font></font></li>
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">训练在固定数量的 epoch 后停止但没有改善</font></font></li>
</ul>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">预训练的单词模型是在 GTX 1050 Ti 上使用以下命令进行训练的：</font></font></p>
<div class="snippet-clipboard-content notranslate position-relative overflow-auto"><pre class="notranslate"><code>python main.py --mode train --fast --data_dir path/to/iam  --batch_size 500 --early_stopping 15
</code></pre><div class="zeroclipboard-container">
    <clipboard-copy aria-label="Copy" class="ClipboardButton btn btn-invisible js-clipboard-copy m-2 p-0 tooltipped-no-delay d-flex flex-justify-center flex-items-center" data-copy-feedback="Copied!" data-tooltip-direction="w" value="python main.py --mode train --fast --data_dir path/to/iam  --batch_size 500 --early_stopping 15" tabindex="0" role="button">
      <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-copy js-clipboard-copy-icon">
    <path d="M0 6.75C0 5.784.784 5 1.75 5h1.5a.75.75 0 0 1 0 1.5h-1.5a.25.25 0 0 0-.25.25v7.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-1.5a.75.75 0 0 1 1.5 0v1.5A1.75 1.75 0 0 1 9.25 16h-7.5A1.75 1.75 0 0 1 0 14.25Z"></path><path d="M5 1.75C5 .784 5.784 0 6.75 0h7.5C15.216 0 16 .784 16 1.75v7.5A1.75 1.75 0 0 1 14.25 11h-7.5A1.75 1.75 0 0 1 5 9.25Zm1.75-.25a.25.25 0 0 0-.25.25v7.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-7.5a.25.25 0 0 0-.25-.25Z"></path>
</svg>
      <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-check js-clipboard-check-icon color-fg-success d-none">
    <path d="M13.78 4.22a.75.75 0 0 1 0 1.06l-7.25 7.25a.75.75 0 0 1-1.06 0L2.22 9.28a.751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018L6 10.94l6.72-6.72a.75.75 0 0 1 1.06 0Z"></path>
</svg>
    </clipboard-copy>
  </div></div>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">以及线路模型：</font></font></p>
<div class="snippet-clipboard-content notranslate position-relative overflow-auto"><pre class="notranslate"><code>python main.py --mode train --fast --data_dir path/to/iam  --batch_size 250 --early_stopping 10
</code></pre><div class="zeroclipboard-container">
    <clipboard-copy aria-label="Copy" class="ClipboardButton btn btn-invisible js-clipboard-copy m-2 p-0 tooltipped-no-delay d-flex flex-justify-center flex-items-center" data-copy-feedback="Copied!" data-tooltip-direction="w" value="python main.py --mode train --fast --data_dir path/to/iam  --batch_size 250 --early_stopping 10" tabindex="0" role="button">
      <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-copy js-clipboard-copy-icon">
    <path d="M0 6.75C0 5.784.784 5 1.75 5h1.5a.75.75 0 0 1 0 1.5h-1.5a.25.25 0 0 0-.25.25v7.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-1.5a.75.75 0 0 1 1.5 0v1.5A1.75 1.75 0 0 1 9.25 16h-7.5A1.75 1.75 0 0 1 0 14.25Z"></path><path d="M5 1.75C5 .784 5.784 0 6.75 0h7.5C15.216 0 16 .784 16 1.75v7.5A1.75 1.75 0 0 1 14.25 11h-7.5A1.75 1.75 0 0 1 5 9.25Zm1.75-.25a.25.25 0 0 0-.25.25v7.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-7.5a.25.25 0 0 0-.25-.25Z"></path>
</svg>
      <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-check js-clipboard-check-icon color-fg-success d-none">
    <path d="M13.78 4.22a.75.75 0 0 1 0 1.06l-7.25 7.25a.75.75 0 0 1-1.06 0L2.22 9.28a.751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018L6 10.94l6.72-6.72a.75.75 0 0 1 1.06 0Z"></path>
</svg>
    </clipboard-copy>
  </div></div>
<h3 tabindex="-1" dir="auto"><a id="user-content-fast-image-loading" class="anchor" aria-hidden="true" tabindex="-1" href="#fast-image-loading"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">快速图像加载</font></font></h3>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">即使仅使用小型 GPU，从磁盘加载和解码 png 图像文件也是瓶颈。</font><font style="vertical-align: inherit;">数据库LMDB用于加速图像加载：</font></font></p>
<ul dir="auto">
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">转到该目录并</font><font style="vertical-align: inherit;">使用指定的 IAM 数据目录</font></font><code>src</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">运行</font></font><code>create_lmdb.py --data_dir path/to/iam</code><font style="vertical-align: inherit;"></font></li>
<li><font style="vertical-align: inherit;"></font><code>lmdb</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">将在 IAM 数据目录中创建</font><font style="vertical-align: inherit;">一个包含 LMDB 文件的子文件夹</font></font></li>
<li><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">训练模型时，添加命令行选项</font></font><code>--fast</code></li>
</ul>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">数据集应位于 SSD 驱动器上。</font><font style="vertical-align: inherit;">使用该</font></font><code>--fast</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">选项和 GTX 1050 Ti 对单个单词进行训练大约需要 3 小时，批量大小为 500。对文本行进行训练需要更长的时间。</font></font></p>
<h2 tabindex="-1" dir="auto"><a id="user-content-information-about-model" class="anchor" aria-hidden="true" tabindex="-1" href="#information-about-model"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">型号信息</font></font></h2>
<p dir="auto"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">该模型是我为论文实现的 HTR 系统的精简版本。</font><font style="vertical-align: inherit;">剩下的就是以可接受的精度识别文本的最低限度。</font><font style="vertical-align: inherit;">它由 5 个 CNN 层、2 个 RNN (LSTM) 层以及 CTC 损失和解码层组成。</font><font style="vertical-align: inherit;">有关更多详细信息，请参阅这篇</font></font><a href="https://towardsdatascience.com/2326a3487cd5" rel="nofollow"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">Medium 文章</font></font></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">。</font></font></p>
<h2 tabindex="-1" dir="auto"><a id="user-content-references" class="anchor" aria-hidden="true" tabindex="-1" href="#references"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">参考</font></font></h2>
<ul dir="auto">
<li><a href="https://towardsdatascience.com/2326a3487cd5" rel="nofollow"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">使用 TensorFlow 构建手写文本识别系统</font></font></a></li>
<li><a href="https://repositum.tuwien.ac.at/obvutwhs/download/pdf/2874742" rel="nofollow"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">Scheidl - 历史文献中的手写文本识别</font></font></a></li>
<li><a href="https://repositum.tuwien.ac.at/obvutwoa/download/pdf/2774578" rel="nofollow"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">Scheidl - Word Beam 搜索：联结主义时间分类解码算法</font></font></a></li>
</ul>
</article></div>

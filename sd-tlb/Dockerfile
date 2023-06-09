FROM runpod/stable-diffusion:models-1.0.0 as sd-models
FROM runpod/stable-diffusion-models:2.1 as hf-cache
FROM runpod/pytorch:3.10-2.0.0-117 as runtime

RUN mkdir -p /root/.cache/huggingface && mkdir /auto-models

COPY --from=hf-cache /root/.cache/huggingface /root/.cache/huggingface
COPY --from=sd-models /SDv1-5.ckpt /auto-models/SDv1-5.ckpt
COPY --from=sd-models /SDv2-768.ckpt /auto-models/SDv2-768.ckpt

ENV PYTHONUNBUFFERED=1

RUN wget https://github.com/TheLastBen/fast-stable-diffusion/raw/main/Dependencies/zstd_1.4.4+dfsg-3ubuntu0.1_amd64.deb && \
    wget https://github.com/TheLastBen/fast-stable-diffusion/raw/main/Dependencies/git-lfs_2.3.4-1_amd64.deb && \
    wget https://github.com/TheLastBen/fast-stable-diffusion/raw/main/Dependencies/rename_1.10-1_all.deb && \
    wget https://github.com/TheLastBen/fast-stable-diffusion/raw/main/Dependencies/zip_3.0-11build1_amd64.deb && \
    wget https://github.com/TheLastBen/fast-stable-diffusion/raw/main/Dependencies/unzip_6.0-25ubuntu1.1_amd64.deb && \
    apt update && apt install -y --no-install-recommends libfontconfig1 libgles2-mesa-dev && dpkg -i *.deb && rm -rf *.deb

RUN pip install gdown && pip install xformers==0.0.18

WORKDIR /workspace

ADD start.sh /start.sh
RUN chmod a+x /start.sh

ADD jupyter_lab_config.py /root/.jupyter/jupyter_lab_config.py

SHELL ["/bin/bash", "--login", "-c"]
CMD [ "/start.sh" ]

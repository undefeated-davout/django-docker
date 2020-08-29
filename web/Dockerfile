FROM centos:7

ARG PYTHON_VERSION='3.7.2'
ARG HOME=/root
ENV PYTHONUNBUFFERED 1

# 環境変数はAnsible設定するが、Docker用にも環境変数を用意する
ENV HOME ${HOME}
ENV PYENV_ROOT $HOME/.pyenv
ENV PATH $PYENV_ROOT/bin:$PYENV_ROOT/versions/${PYTHON_VERSION}/bin:$PATH

# Ansible実行に必要なライブラリをインストール
RUN yum -y update && \
    yum -y install \
            git \
            ansible

# webディレクトリ配下のファイルをworkへコピーしAnsible実行
WORKDIR /work
ADD . /work/
RUN ansible-playbook ansible-django/deploy.yml

# ホストとのコード共有用ディレクトリ作成
WORKDIR /code

CMD ["/bin/bash"]

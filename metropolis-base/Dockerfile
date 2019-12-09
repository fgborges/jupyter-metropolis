FROM fedora:31 as scala
RUN dnf install -y java-11-openjdk-devel python3-notebook
RUN useradd -m jovyan
USER jovyan
WORKDIR /home/jovyan
ENV USER=jovyan HOME=/home/jovyan PATH=/home/jovyan/.local/bin:$PATH LANG=C.UTF-8
RUN curl -fsSLo ./coursier https://git.io/coursier-cli \
    && chmod +x ./coursier \
    && ./coursier bootstrap --standalone -r jitpack -i user -I user:sh.almond:scala-kernel-api_2.13.1:0.8.2 sh.almond:scala-kernel_2.13.1:0.8.2 -o ./almond \
    && ./almond --install

FROM fedora:31 as java
RUN dnf install -y java-11-openjdk-devel python3-notebook
RUN dnf install -y bsdtar
RUN useradd -m jovyan
USER jovyan
WORKDIR /home/jovyan
ENV USER=jovyan HOME=/home/jovyan PATH=/home/jovyan/.local/bin:$PATH LANG=C.UTF-8
RUN curl -fsSL https://github.com/SpencerPark/IJava/releases/download/v1.3.0/ijava-1.3.0.zip | bsdtar -xf - \
    && python3 install.py --user

FROM fedora:31 as clojure
RUN dnf install -y java-11-openjdk-devel python3-notebook
RUN dnf install -y make which ImageMagick
RUN useradd -m jovyan
USER jovyan
WORKDIR /home/jovyan
ENV USER=jovyan HOME=/home/jovyan PATH=/home/jovyan/.local/bin:/home/jovyan:$PATH LANG=C.UTF-8
RUN curl -fsSLo /home/jovyan/lein https://raw.githubusercontent.com/technomancy/leiningen/stable/bin/lein \
    && chmod +x /home/jovyan/lein \
    && git clone --depth=1 git://github.com/clojupyter/clojupyter \
    && make -C clojupyter install

FROM fedora:31 as groovy
RUN dnf install -y java-1.8.0-openjdk-devel python3-notebook
RUN useradd -m jovyan
USER jovyan
WORKDIR /home/jovyan
ENV USER=jovyan HOME=/home/jovyan PATH=/home/jovyan/.local/bin:$PATH LANG=C.UTF-8 JAVA_HOME=/usr/lib/jvm/java
RUN git clone --depth=1 git://github.com/lappsgrid-incubator/jupyter-groovy-kernel \
    && cd jupyter-groovy-kernel \
    && ./mvnw clean package \
    && mkdir -p $HOME/.local/share/jupyter/kernels/groovy \
    && sed -i 's%cp target/$JAR $KERNEL_DIR%cp target/$JAR $DIST%' ./install.sh \
    && ./install.sh $HOME/.local/share/jupyter/kernels/groovy

FROM fedora:31 as kotlin
RUN dnf install -y java-1.8.0-openjdk-devel python3-notebook
RUN useradd -m jovyan
USER jovyan
WORKDIR /home/jovyan
ENV USER=jovyan HOME=/home/jovyan PATH=/home/jovyan/.local/bin:$PATH LANG=C.UTF-8 JAVA_HOME=/usr/lib/jvm/java 
RUN git clone --depth=1 git://github.com/ligee/kotlin-jupyter \
    && cd kotlin-jupyter \
    && ./gradlew install

FROM fedora:31 as common-lisp
RUN dnf install -y @c-development @development-tools zeromq-devel python3-notebook
RUN dnf install -y sbcl rpm-build
RUN useradd -m jovyan
USER jovyan
WORKDIR /home/jovyan
ENV USER=jovyan HOME=/home/jovyan PATH=/home/jovyan/.local/bin:$PATH LANG=C.UTF-8
RUN curl -fsSLO https://beta.quicklisp.org/quicklisp.lisp \
    && sbcl --load quicklisp.lisp --eval '(quicklisp-quickstart:install)' --quit \
    && rm quicklisp.lisp \
    && echo '(load (merge-pathnames "quicklisp/setup.lisp" (user-homedir-pathname)))' >> $HOME/.sbclrc
RUN sbcl --eval '(ql:quickload :common-lisp-jupyter)' --eval '(cl-jupyter:install :local t)' --quit

FROM fedora:31 as julia
RUN dnf install -y @c-development @development-tools zeromq-devel python3-notebook
RUN dnf install -y julia
RUN useradd -m jovyan
USER jovyan
WORKDIR /home/jovyan
ENV USER=jovyan HOME=/home/jovyan PATH=/home/jovyan/.local/bin:$PATH LANG=C.UTF-8
RUN julia -e 'using Pkg; Pkg.add("IJulia")'

FROM fedora:31 as rust
RUN dnf install -y @c-development @development-tools zeromq-devel python3-notebook
RUN dnf install -y cargo cmake
RUN useradd -m jovyan
USER jovyan
WORKDIR /home/jovyan
ENV USER=jovyan HOME=/home/jovyan PATH=/home/jovyan/.local/bin:$PATH LANG=C.UTF-8
RUN cargo install evcxr_jupyter --no-default-features
RUN $HOME/.cargo/bin/evcxr_jupyter --install

FROM fedora:31 as golang
RUN dnf install -y @c-development @development-tools zeromq-devel python3-notebook
RUN dnf install -y golang
RUN useradd -m jovyan
USER jovyan
WORKDIR /home/jovyan
ENV USER=jovyan HOME=/home/jovyan PATH=/home/jovyan/.local/bin:$PATH LANG=C.UTF-8
RUN go get -u github.com/gopherdata/gophernotes
RUn jupyter kernelspec install --user --name golang $HOME/go/src/github.com/gopherdata/gophernotes/kernel

FROM fedora:31 as racket
RUN dnf install -y @c-development @development-tools zeromq-devel python3-notebook
RUN dnf install -y sqlite-devel
RUN useradd -m jovyan
USER jovyan
WORKDIR /home/jovyan
ENV USER=jovyan HOME=/home/jovyan PATH=/home/jovyan/.local/bin:$PATH LANG=C.UTF-8
RUN curl -fsSLo installer.sh https://mirror.racket-lang.org/installers/7.4/racket-7.4-x86_64-linux-cs.sh \
    && bash installer.sh --unix-style --dest $HOME/racket --create-dir \
    && rm installer.sh
ENV PATH=/home/jovyan/racket/bin:$PATH
RUN raco pkg install --auto --batch --skip-installed iracket
RUN raco iracket install

FROM fedora:31 as raku
RUN dnf install -y @c-development @development-tools zeromq-devel python3-notebook
RUN dnf install -y rakudo-zef libatomic_ops-devel libuv-devel libtommath-devel rpm-build libffi-devel
RUN useradd -m jovyan
USER jovyan
WORKDIR /home/jovyan
ENV USER=jovyan HOME=/home/jovyan PATH=/home/jovyan/.local/bin:$PATH LANG=C.UTF-8
RUN zef install Jupyter::Kernel
RUN $HOME/.perl6/bin/jupyter-kernel.p6 --generate-config

FROM fedora:31 as ruby
RUN dnf install -y @c-development @development-tools zeromq-devel python3-notebook
RUN dnf install -y ruby-devel rubygems rubygem-ffi
RUN useradd -m jovyan
USER jovyan
WORKDIR /home/jovyan
ENV USER=jovyan HOME=/home/jovyan PATH=/home/jovyan/.local/bin:$PATH LANG=C.UTF-8
RUN mkdir -p $HOME/.local/bin \
    && gem install --user-install --bindir=$HOME/.local/bin ffi-rzmq pry iruby
RUN iruby register

FROM fedora:31 as haskell
RUN dnf install -y @c-development @development-tools zeromq-devel python3-notebook
RUN dnf install -y xz ncurses-devel gmp-devel bsdtar zlib-devel
RUN useradd -m jovyan
USER jovyan
WORKDIR /home/jovyan
ENV USER=jovyan HOME=/home/jovyan PATH=/home/jovyan/.local/bin:$PATH LANG=C.UTF-8
RUN mkdir -p $HOME/.local/bin \
    && curl -fsSL https://github.com/commercialhaskell/stack/releases/download/v2.1.3/stack-2.1.3-linux-x86_64-static.tar.gz | bsdtar -xf - -O */stack > $HOME/.local/bin/stack \
    && chmod +x $HOME/.local/bin/stack
RUN git clone --depth=1 git://github.com/gibiansky/IHaskell
WORKDIR ./IHaskell
RUN sed -i 's%- ./ihaskell-display%# - ./ihaskell-display%' ./stack.yaml
RUN stack install --fast
RUN ihaskell install --stack

FROM fedora:31 as nodejs
RUN dnf install -y @c-development @development-tools zeromq-devel python3-notebook
RUN dnf install -y npm zeromq
RUN useradd -m jovyan
USER jovyan
WORKDIR /home/jovyan
ENV USER=jovyan HOME=/home/jovyan PATH=/home/jovyan/.local/bin:$PATH LANG=C.UTF-8
RUN npm config set prefix $HOME/node_modules \
    && npm install -g tslab \
    && $HOME/node_modules/bin/tslab install

FROM fedora:31 as ocaml
RUN dnf install -y @c-development @development-tools zeromq-devel python3-notebook
RUN dnf install -y opam diffutils xz gmp-devel zlib-devel
RUN useradd -m jovyan
USER jovyan
WORKDIR /home/jovyan
ENV USER=jovyan HOME=/home/jovyan PATH=/home/jovyan/.local/bin:$PATH LANG=C.UTF-8
RUN opam init --disable-sandboxing --compiler=4.08.1
RUN opam install -y jupyter
RUN jupyter kernelspec install --user --name ocaml $(opam config var share)/jupyter

FROM fedora:31 as elixir
RUN dnf install -y @c-development @development-tools zeromq-devel python3-notebook
RUN dnf install -y elixir erlang sqlite-devel
RUN useradd -m jovyan
USER jovyan
WORKDIR /home/jovyan
ENV USER=jovyan HOME=/home/jovyan PATH=/home/jovyan/.local/bin:$PATH LANG=C.UTF-8
RUN git clone --depth=1 git://github.com/pprzetacznik/IElixir
WORKDIR ./IElixir
RUN mix local.rebar --force
RUN mix local.hex --force
RUN mix deps.get
RUN mix test
RUN MIX_ENV=prod mix compile
RUN sed -i 's/python /python3 /' ./install_script.sh
RUN ./install_script.sh

FROM fedora:31
RUN dnf install -y @c-development @development-tools zeromq-devel python3-notebook
RUN rpm --import https://packages.microsoft.com/keys/microsoft.asc \
    && curl -fsSLo /etc/yum.repos.d/microsoft-prod.repo https://packages.microsoft.com/config/rhel/7/prod.repo \
    && dnf install -y \
        julia \
        java-11-openjdk-devel \
        dotnet-sdk-3.1 \
        sagemath \
        npm \
        cargo \
        golang \
        python3-jupyroot \
        root \
        opam \
        coq-coqide \
        Agda \
        octave \
        gnuplot \
        compat-openssl10 \
        powershell \
        sbcl \
        pari-devel \
        Singular-devel \
        vim \
        pl \
        lua \
        elixir \
        erlang \
        R-devel \
        R-IRkernel \
        perl-App-cpanminus \
        rakudo-zef \
        ruby-devel \
        rubygems \
        rubygem-ffi \
        python3-pip \
        gcc-gfortran \
        jq \
        ImageMagick \
        zlib-devel \
        gmp-devel \
        readline-devel \
        zeromq-devel \
        ncurses-devel \
        sqlite-devel \
        libatomic_ops-devel \
        libuv-devel \
        libtommath-devel \
    && dnf clean all
RUN jq '.argv[0]="/usr/bin/sage"' /usr/share/jupyter/kernels/sagemath/kernel.json > /tmp/kernel.json \
    && mv /tmp/kernel.json /usr/share/jupyter/kernels/sagemath/kernel.json

RUN useradd -m jovyan
USER jovyan
WORKDIR /home/jovyan
ENV USER=jovyan HOME=/home/jovyan PATH=/home/jovyan/.local/bin:$PATH LANG=C.UTF-8
RUN mkdir -p $HOME/.local/bin $HOME/.local/share/jupyter/kernels

ENV PATH=$HOME/.dotnet/tools:$PATH
RUN dotnet tool install -g dotnet-interactive --add-source "https://dotnet.myget.org/F/dotnet-try/api/v3/index.json" \
    && dotnet interactive jupyter install

RUN pip3 install --user coq-jupyter \
    && python3 -m coq_jupyter.install

RUN pip3 install --user bash_kernel \
    && python3 -m bash_kernel.install

RUN pip3 install --user agda_kernel \
    && python3 -m agda_kernel.install

RUN pip3 install --user ilua

RUN pip install --upgrade --user jswipl \
    && mkdir -p $HOME/.local/share/jupyter/kernels/prolog \
    && curl -fsSLo $HOME/.local/share/jupyter/kernels/prolog/kernel.json https://raw.githubusercontent.com/targodan/jupyter-swi-prolog/master/kernel.json

RUN pip3 install --user octave_kernel

RUN pip3 install --user pari_jupyter

RUN git clone --depth=1 git://github.com/ZedThree/jupyter-fortran-kernel \
    && pip3 install --user -e ./jupyter-fortran-kernel \
    && jupyter kernelspec install --user ./jupyter-fortran-kernel/fortran_spec

RUN pip3 install --user jupyter-c-kernel \
    && install_c_kernel --user

RUN pip3 install --user powershell_kernel \
    && python3 -m powershell_kernel.install

RUN pip3 install --user PySingular jupyter_kernel_singular

RUN git clone --depth=1 git://github.com/mattn/vim_kernel \
    && cd vim_kernel \
    && python3 setup.py install --user \
    && python3 -m vim_kernel.install

RUN cpanm Devel::IPerl --notest \
    && cpanm --local-lib=~/perl5 local::lib && eval $(perl -I ~/perl5/lib/perl5/ -Mlocal::lib) \
    && iperl --version

COPY --chown=jovyan:jovyan --from=scala /home/jovyan/.local/ $HOME/.local/
COPY --chown=jovyan:jovyan --from=kotlin /home/jovyan/.ipython/ $HOME/.local/share/jupyter/
COPY --chown=jovyan:jovyan --from=clojure /home/jovyan/.local/ $HOME/.local/
COPY --chown=jovyan:jovyan --from=java /home/jovyan/.local/ $HOME/.local/
COPY --chown=jovyan:jovyan --from=groovy /home/jovyan/.local/ $HOME/.local/

ENV PATH=$HOME/node_modules/bin:$PATH
COPY --chown=jovyan:jovyan --from=nodejs /home/jovyan/.npmrc  $HOME/.npmrc
COPY --chown=jovyan:jovyan --from=nodejs /home/jovyan/.local/ $HOME/.local/
COPY --chown=jovyan:jovyan --from=nodejs /home/jovyan/node_modules/ $HOME/node_modules/

COPY --chown=jovyan:jovyan --from=haskell /home/jovyan/.local/ $HOME/.local/
COPY --chown=jovyan:jovyan --from=haskell /home/jovyan/.stack/ $HOME/.stack/

COPY --chown=jovyan:jovyan --from=ocaml /home/jovyan/.opam/ $HOME/.opam/
COPY --chown=jovyan:jovyan --from=ocaml /home/jovyan/.local/ $HOME/.local/

ENV PATH=$HOME/.cargo/bin:$PATH
COPY --chown=jovyan:jovyan --from=rust /home/jovyan/.cargo/ $HOME/.cargo/
COPY --chown=jovyan:jovyan --from=rust /home/jovyan/.local/ $HOME/.local/

ENV PATH=$HOME/go/bin:$PATH
COPY --chown=jovyan:jovyan --from=golang /home/jovyan/go/ $HOME/go/
COPY --chown=jovyan:jovyan --from=golang /home/jovyan/.local/ $HOME/.local/

ENV PATH=$HOME/mix:$PATH
COPY --chown=jovyan:jovyan --from=elixir /home/jovyan/.hex/ $HOME/.hex/
COPY --chown=jovyan:jovyan --from=elixir /home/jovyan/.mix/ $HOME/.mix/
COPY --chown=jovyan:jovyan --from=elixir /home/jovyan/IElixir/ $HOME/IElixir/
COPY --chown=jovyan:jovyan --from=elixir /home/jovyan/.local/ $HOME/.local/

COPY --chown=jovyan:jovyan --from=julia /home/jovyan/.julia/ $HOME/.julia/
COPY --chown=jovyan:jovyan --from=julia /home/jovyan/.local/ $HOME/.local/

COPY --chown=jovyan:jovyan --from=common-lisp /home/jovyan/quicklisp/ $HOME/quicklisp/
COPY --chown=jovyan:jovyan --from=common-lisp /home/jovyan/.sbclrc $HOME/.sbclrc
COPY --chown=jovyan:jovyan --from=common-lisp /home/jovyan/.local/ $HOME/.local/

ENV PATH=$HOME/.perl6/bin:$PATH
COPY --chown=jovyan:jovyan --from=raku /home/jovyan/.zef/ $HOME/.zef/
COPY --chown=jovyan:jovyan --from=raku /home/jovyan/.perl6/ $HOME/.perl6/
COPY --chown=jovyan:jovyan --from=raku /home/jovyan/.local/ $HOME/.local/

ENV PATH=$HOME/racket/bin:$PATH
COPY --chown=jovyan:jovyan --from=racket /home/jovyan/.racket/  $HOME/.racket/
COPY --chown=jovyan:jovyan --from=racket /home/jovyan/racket/ $HOME/racket/
COPY --chown=jovyan:jovyan --from=racket /home/jovyan/.local/ $HOME/.local/

COPY --chown=jovyan:jovyan --from=ruby /home/jovyan/.gem/ $HOME/.gem/
COPY --chown=jovyan:jovyan --from=ruby /home/jovyan/.local/ $HOME/.local/

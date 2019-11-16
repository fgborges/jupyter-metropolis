FROM fedora:31
RUN echo 'max_parallel_downloads=10' >> /etc/dnf/dnf.conf \
    && rpm --import https://packages.microsoft.com/keys/microsoft.asc \
    && curl -fsSLo /etc/yum.repos.d/microsoft-prod.repo https://packages.microsoft.com/config/rhel/7/prod.repo \
    && dnf install -y \
        @c-development \
        @development-tools \
        julia \
        java-11-openjdk \
        dotnet-sdk-3.0 \
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
        rakudo-zef \
        ruby-devel \
        rubygems \
        python3-pip \
        python3-notebook \
        gcc-gfortran \
        jq \
        xz \
        diffutils \
        bsdtar\
        cmake \
        ImageMagick \
        gmp-devel \
        readline-devel \
        zeromq-devel \
        ncurses-devel \
        sqlite-devel \
        libatomic_ops-devel \
        libuv-devel \
        libtommath-devel \
        perl-App-cpanminus \
    && dnf clean all
RUN jq '.argv[0]="/usr/bin/sage"' /usr/share/jupyter/kernels/sagemath/kernel.json > /tmp/kernel.json \
    && mv /tmp/kernel.json /usr/share/jupyter/kernels/sagemath/kernel.json

RUN useradd -m jovyan
USER jovyan
WORKDIR /home/jovyan
ENV USER=jovyan HOME=/home/jovyan PATH=/home/jovyan/.local/bin:$PATH LANG=C.UTF-8
RUN mkdir -p $HOME/.local/bin $HOME/.local/share/jupyter/kernels

RUN curl -fsSL https://github.com/commercialhaskell/stack/releases/download/v2.1.3/stack-2.1.3-linux-x86_64-static.tar.gz | bsdtar -xf - -O */stack > $HOME/.local/bin/stack \
    && chmod +x $HOME/.local/bin/stack \
    && git clone --depth=1 git://github.com/gibiansky/IHaskell \
    && cd IHaskell \
    && sed -i 's/==.*$//' requirements.txt \
    && pip3 install --user -r requirements.txt \
    && sed -i 's%- ./ihaskell-display%# - ./ihaskell-display%' ./stack.yaml \
    && stack install --fast \
    && ihaskell install --stack \
    && cd $HOME \
    && rm -rf $OLDPWD

ENV PATH=$HOME/.mix:$PATH
RUN git clone --depth=1 git://github.com/pprzetacznik/IElixir \
    && cd IElixir \
    && mix local.rebar --force \
    && mix local.hex --force \
    && mix deps.get \
    && mix test \
    && MIX_ENV=prod mix compile \
    && sed -i 's/python /python3 /' ./install_script.sh \
    && ./install_script.sh

ENV PATH=$HOME/.dotnet/tools:$PATH
RUN dotnet tool install -g dotnet-try \
    && dotnet try jupyter install

RUN opam init --disable-sandboxing --compiler=4.08.1 \
    && opam install -y jupyter \
    && jupyter kernelspec install --user $(opam config var share)/jupyter

RUN pip3 install --user coq-jupyter \
    && python3 -m coq_jupyter.install

ENV PATH=$HOME/go/bin:$PATH
RUN go get -u github.com/gopherdata/gophernotes \
    && jupyter kernelspec install --user $HOME/go/src/github.com/gopherdata/gophernotes/kernel

ENV PATH=$HOME/.cargo/bin:$PATH
RUN cargo install evcxr_jupyter --no-default-features \
    && evcxr_jupyter --install

RUN curl -fsSLO https://beta.quicklisp.org/quicklisp.lisp \
    && sbcl --load quicklisp.lisp --eval '(quicklisp-quickstart:install)' --quit \
    && rm quicklisp.lisp \
    && echo '(load (merge-pathnames "quicklisp/setup.lisp" (user-homedir-pathname)))' >> $HOME/.sbclrc \
    && sbcl --eval '(ql:quickload :common-lisp-jupyter)' --eval '(cl-jupyter:install)' --quit

RUN julia -e 'using Pkg; Pkg.add("IJulia")'

RUN cd $(mktemp -d) \
    && curl -fsSL https://github.com/SpencerPark/IJava/releases/download/v1.3.0/ijava-1.3.0.zip | bsdtar -xf - \
    && python3 install.py --user \
    && cd $HOME \
    && rm -rf $OLDPWD

RUN cd $(mktemp -d) \
    && curl -fsSLo $HOME/.local/bin/coursier https://git.io/coursier-cli \
    && chmod +x $HOME/.local/bin/coursier \
    && coursier bootstrap --standalone -r jitpack -i user -I user:sh.almond:scala-kernel-api_2.13.1:0.8.2 sh.almond:scala-kernel_2.13.1:0.8.2 -o $HOME/.local/bin/almond \
    && almond --install \
    && cd $HOME \
    && rm -rf $OLDPWD

RUN curl -fsSLo $HOME/.local/bin/lein https://raw.githubusercontent.com/technomancy/leiningen/stable/bin/lein \
    && chmod +x $HOME/.local/bin/lein \
    && git clone --depth=1 git://github.com/clojupyter/clojupyter \
    && make -C clojupyter install

RUN git clone --depth=1 git://github.com/ligee/kotlin-jupyter \
    && cd kotlin-jupyter \
    && ./gradlew install

RUN git clone --depth=1 git://github.com/lappsgrid-incubator/jupyter-groovy-kernel \
    && cd jupyter-groovy-kernel \
    && ./mvnw clean package \
    && mkdir -p $HOME/.local/share/jupyter/kernels/groovy \
    && sed -i 's%cp target/$JAR $KERNEL_DIR%cp target/$JAR $DIST%' ./install.sh \
    && ./install.sh $HOME/.local/share/jupyter/kernels/groovy

ENV PATH=$HOME/racket/bin:$PATH
RUN curl -fsSLo installer.sh https://mirror.racket-lang.org/installers/7.4/racket-7.4-x86_64-linux-cs.sh \
    && bash installer.sh --unix-style --dest $HOME/racket --create-dir \
    && rm installer.sh \
    && raco pkg install --auto --batch --skip-installed iracket \
    && raco iracket install

ENV PATH=$HOME/node_modules/bin:$PATH
RUN npm config set prefix $HOME/node_modules \
    && npm install -g tslab \
    && tslab install

RUN gem install --user-install --bindir $HOME/.local/bin ffi-rzmq iruby \
    && iruby register

ENV PATH=$HOME/.perl6/bin:$PATH
RUN zef install Jupyter::Kernel \
    && jupyter-kernel.p6 --generate-config

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

RUN cpanm Devel::IPerl --notest \
    && cpanm --local-lib=~/perl5 local::lib && eval $(perl -I ~/perl5/lib/perl5/ -Mlocal::lib) \
    && iperl --version

RUN pip3 install --user PySingular jupyter_kernel_singular

RUN git clone --depth=1 git://github.com/mattn/vim_kernel \
    && cd vim_kernel \
    && python3 setup.py install --user \
    && python3 -m vim_kernel.install

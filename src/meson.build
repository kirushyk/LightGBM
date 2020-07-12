lightgbm_sources = [
    'treelearner/serial_tree_learner.cpp',
    'treelearner/voting_parallel_tree_learner.cpp',
    'treelearner/feature_parallel_tree_learner.cpp',
    'treelearner/data_parallel_tree_learner.cpp',
    'treelearner/tree_learner.cpp',
    'treelearner/gpu_tree_learner.cpp',
    'network/network.cpp',
    'network/linker_topo.cpp',
    'network/linkers_socket.cpp',
    'network/linkers_mpi.cpp',
    'io/dataset_loader.cpp',
    'io/bin.cpp',
    'io/tree.cpp',
    'io/file_io.cpp',
    'io/config.cpp',
    'io/config_auto.cpp',
    'io/json11.cpp',
    'io/metadata.cpp',
    'io/dataset.cpp',
    'io/parser.cpp',
    'objective/objective_function.cpp',
    'c_api.cpp',
    'boosting/gbdt_model_text.cpp',
    'boosting/gbdt.cpp',
    'boosting/gbdt_prediction.cpp',
    'boosting/prediction_early_stop.cpp',
    'boosting/boosting.cpp',
    'application/application.cpp',
    'metric/dcg_calculator.cpp',
    'metric/metric.cpp'
]

lightgbm_cflags = []

cxx = meson.get_compiler('cpp')

mm_malloc_check = '''
#include <mm_malloc.h>
int main() {
  char *a = (char*)_mm_malloc(8, 16);
  _mm_free(a);
  return 0;
}
'''

if cxx.links(mm_malloc_check, name: '_mm_malloc')
    lightgbm_cflags += ['-DMM_MALLOC']
endif

mm_prefetch_check = '''
#include <xmmintrin.h>
int main() {
  int a = 0;
  _mm_prefetch(&a, _MM_HINT_NTA);
  return 0;
}
'''

if cxx.links(mm_prefetch_check, name: '_mm_prefetch')
    lightgbm_cflags += ['-DMM_PREFETCH']
endif

mpi = dependency('mpi', language: 'cpp', required: get_option('mpi'))
use_mpi = mpi.found()

if use_mpi
    lightgbm_cflags += ['-DUSE_MPI']
else
    lightgbm_cflags += ['-DUSE_SOCKET']
endif

lightgbm = library('_lightgbm', lightgbm_sources,
    include_directories: inc,
    cpp_args: lightgbm_cflags
)

executable('lightgbm', 'main.cpp',
    include_directories: inc,
    cpp_args: lightgbm_cflags,
    link_with: lightgbm
)

# ============================================================================
# FEM MOR simulation build system — Wine-based solver pipeline
# ============================================================================

# --- Configuration (override with make VAR=value or via environment) ---
WINE         ?= wine
WINEDEBUG    ?= -all
FEM_SOLVERS  := $(abspath FEM_MOR_projects/femSolvers)
WINE_PATH    := env WINEPATH="$(FEM_SOLVERS)"

MESH_READER  := env WINEDEBUG="$(WINEDEBUG)" $(WINE) "$(FEM_SOLVERS)/ANST_MeshReader.exe"
WAVE_SOLVER  := env WINEDEBUG="$(WINEDEBUG)" $(WINE_PATH) $(WINE) "$(FEM_SOLVERS)/EM_WaveSolver.exe"

# Project directories
PRJ_1x1vivaldi := FEM_MOR_projects/1x1vivaldi_LTE
PRJ_3x5_1st    := FEM_MOR_projects/3x5patch_1stOrder
PRJ_3x5_2nd    := FEM_MOR_projects/3x5patch_2ndOrder
PRJ_40vivaldi  := FEM_MOR_projects/40vivaldi

# Frequencies
FREQ_1x1vivaldi := 7.5e9
FREQ_3x5patch   := 2.35e9
FREQ_40vivaldi  := 3e9

# Solver flags
#   +singleEnded  port-by-port processing
#   +direct       assemble and solve natively (no MATLAB)
#   +sol          write solution vectors and Touchstone S-parameter
#   +fieldValues  compute fields at evaluation points
SOLVE := +singleEnded +direct +sol +fieldValues

.PHONY: all help list check-wine
.PHONY: mesh-1x1vivaldi mesh-3x5-1st mesh-3x5-2nd mesh-40vivaldi mesh-all
.PHONY: solve-1x1vivaldi solve-3x5-1st solve-3x5-2nd solve-40vivaldi solve-all
.PHONY: functional-1x1vivaldi functional-3x5-1st functional-3x5-2nd functional-40vivaldi functional-all
.PHONY: clean-1x1vivaldi clean-3x5-1st clean-3x5-2nd clean-40vivaldi clean clean-all

# ============================================================================
# Help
# ============================================================================

all: check-wine
	@echo "Available targets:"
	@echo "  mesh-<project>        Mesh via ANST_MeshReader (or unzip for 40vivaldi)"
	@echo "  mesh-all              Mesh all projects"
	@echo "  solve-<project>       Solve (+direct +sol +fieldValues)"
	@echo "  solve-all             Solve all projects"
	@echo "  functional-<project>  Compute functionals (+directMatlab)"
	@echo "  functional-all        Compute functionals for all projects"
	@echo "  full-<project>        mesh + solve + functional pipeline"
	@echo "  clean-<project>       Remove solver outputs"
	@echo "  clean-all             Clean all projects"
	@echo ""
	@echo "Projects: 1x1vivaldi, 3x5-1st, 3x5-2nd, 40vivaldi"
	@echo ""
	@echo "Examples:"
	@echo "  make full-1x1vivaldi"
	@echo "  make functional-3x5-1st FREQ_3x5patch=3.0e9"

help: all

check-wine:
	@command -v $(WINE) >/dev/null 2>&1 || { \
	  echo "Error: '$(WINE)' not found. Run ./configure or install it manually."; \
	  exit 1; \
	}

# ============================================================================
# Mesh  —  ANST_MeshReader (1x1vivaldi, 3x5patch) or lte_file.zip (40vivaldi)
# ============================================================================

mesh-1x1vivaldi: check-wine
	cd $(PRJ_1x1vivaldi) && \
	  echo "Running ANST_MeshReader..." && \
	  $(MESH_READER) 1x1vivaldi_LTE && \
	  cp FieldEvaluationPoints.fmat *.mpara *.seinfo lte_fileset/ 2>/dev/null; true

mesh-3x5-1st: check-wine
	cd $(PRJ_3x5_1st) && \
	  echo "Running ANST_MeshReader..." && \
	  $(MESH_READER) 3x5patch_1stOrder && \
	  cp FieldEvaluationPoints.fmat *.mpara *.seinfo lte_fileset/ 2>/dev/null; true

mesh-3x5-2nd: check-wine
	cd $(PRJ_3x5_2nd) && \
	  echo "Running ANST_MeshReader..." && \
	  $(MESH_READER) 3x5patch_2ndOrder && \
	  cp FieldEvaluationPoints.fmat *.mpara *.seinfo lte_fileset/ 2>/dev/null; true

mesh-40vivaldi:
	cd $(PRJ_40vivaldi) && \
	  echo "Extracting lte_file.zip..." && \
	  unzip -o lte_file.zip -d lte_fileset/ && \
	  cp FieldEvaluationPoints.fmat *.mpara *.seinfo lte_fileset/ 2>/dev/null; true

mesh-all: mesh-1x1vivaldi mesh-3x5-1st mesh-3x5-2nd mesh-40vivaldi

# ============================================================================
# Solve  —  +singleEnded +direct +sol +fieldValues
# ============================================================================

solve-1x1vivaldi: check-wine
	cd $(PRJ_1x1vivaldi)/lte_fileset; \
	  echo "Solving system..." && \
	  $(WAVE_SOLVER) 1x1vivaldi_LTE $(FREQ_1x1vivaldi) $(SOLVE)

solve-3x5-1st: check-wine
	cd $(PRJ_3x5_1st)/lte_fileset; \
	  echo "Solving system..." && \
	  $(WAVE_SOLVER) 3x5patch_1stOrder $(FREQ_3x5patch) $(SOLVE)

solve-3x5-2nd: check-wine
	cd $(PRJ_3x5_2nd)/lte_fileset; \
	  echo "Solving system..." && \
	  $(WAVE_SOLVER) 3x5patch_2ndOrder $(FREQ_3x5patch) $(SOLVE)

solve-40vivaldi: check-wine
	cd $(PRJ_40vivaldi)/lte_fileset; \
	  echo "Solving 40vivaldi..." && \
	  $(WAVE_SOLVER) 40vivaldi $(FREQ_40vivaldi) $(SOLVE)

solve-all: solve-1x1vivaldi solve-3x5-1st solve-3x5-2nd solve-40vivaldi

# ============================================================================
# Functional  —  +directMatlab +fieldFunctional
# ============================================================================

functional-1x1vivaldi: check-wine
	cd $(PRJ_1x1vivaldi)/lte_fileset; \
	  echo "Computing functionals..." && \
	  printf '\n\n\n\n\n' | $(WAVE_SOLVER) 1x1vivaldi_LTE $(FREQ_1x1vivaldi) \
	    +directMatlab +fieldFunctional

functional-3x5-1st: check-wine
	cd $(PRJ_3x5_1st)/lte_fileset; \
	  echo "Computing functionals..." && \
	  printf '\n\n\n\n\n' | $(WAVE_SOLVER) 3x5patch_1stOrder $(FREQ_3x5patch) \
	    +singleEnded +directMatlab +fieldFunctional

functional-3x5-2nd: check-wine
	cd $(PRJ_3x5_2nd)/lte_fileset; \
	  echo "Computing functionals..." && \
	  printf '\n\n\n\n\n' | $(WAVE_SOLVER) 3x5patch_2ndOrder $(FREQ_3x5patch) \
	    +singleEnded +directMatlab +fieldFunctional

functional-40vivaldi: check-wine
	cd $(PRJ_40vivaldi)/lte_fileset; \
	  echo "Computing functionals..." && \
	  printf '\n\n\n\n\n' | $(WAVE_SOLVER) 40vivaldi $(FREQ_40vivaldi) \
	    +directMatlab +fieldValues +fieldFunctional

functional-all: functional-1x1vivaldi functional-3x5-1st functional-3x5-2nd functional-40vivaldi

# ============================================================================
# Full pipeline  —  mesh + solve + functional
# ============================================================================

full-1x1vivaldi: mesh-1x1vivaldi solve-1x1vivaldi functional-1x1vivaldi
full-3x5-1st:    mesh-3x5-1st    solve-3x5-1st    functional-3x5-1st
full-3x5-2nd:    mesh-3x5-2nd    solve-3x5-2nd    functional-3x5-2nd
full-40vivaldi:  mesh-40vivaldi  solve-40vivaldi  functional-40vivaldi

# ============================================================================
# Clean
# ============================================================================

clean-1x1vivaldi:
	cd $(PRJ_1x1vivaldi) && rm -rf lte_fileset/ 2>/dev/null; true

clean-3x5-1st:
	cd $(PRJ_3x5_1st) && rm -rf lte_fileset/ 2>/dev/null; true

clean-3x5-2nd:
	cd $(PRJ_3x5_2nd) && rm -rf lte_fileset/ 2>/dev/null; true

clean-40vivaldi:
	cd $(PRJ_40vivaldi) && rm -rf lte_fileset/ 2>/dev/null; true

clean: clean-1x1vivaldi clean-3x5-1st clean-3x5-2nd clean-40vivaldi
	@echo "All solver outputs cleaned."

clean-all: clean

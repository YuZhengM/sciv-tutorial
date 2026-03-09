2.	SCIV usage
=========================


2.1 Install
-------------------------

.. code-block:: shell

    conda create --name sciv python=3.10
    conda activate sciv
    pip install sciv


2.2 Run SCIV
-------------------------

 | Run the code

.. code-block:: python

    # -*- coding: UTF-8 -*-

    import os.path
    import sciv

    if __name__ == '__main__':

        # base path
        base_path: str = "/project/sciv"

        # path
        save_path = f"{base_path}/result/GSE139369_ELM_sim/data"

        # scATAC-seq data
        sc_atac_file = f"{base_path}/input/scATAC/GSE139369_ELM_sim/GSE139369_ELM_sim.h5ad"
        sc_atac = sciv.fl.read_h5ad(file=sc_atac_file)

        # read variant information
        variant_base_path: str = f"{base_path}/input/variant/GSE139369_ELM_sim/hg19"
        variant_column_map: dict = {0: "chr", 1: "position", 3: "rsId", 4: "pp"}
        variants, trait_info = sciv.fl.read_variants(variant_base_path, column_map=variant_column_map)

        # run
        trs = sciv.ml.core(
            adata=sc_atac,
            variants=variants,
            trait_info=trait_info,
            save_path=save_path,
            model_dir=os.path.join(save_path, "poisson_vi"),
            is_simple=False,
            is_ablation=True,
            is_file_exist_loading=True
        )

# Data Engineering Guide
A study guide and roadmap for data engineering topics.

## Tracks
- `data-engineering-for-aws/` (scaffold; content WIP)
- `data-engineering-for-blockchain/` (scaffold; content WIP)

## Repository Outline (Planned)
The folders and files below are the intended structure for the curriculum. Not everything exists yet.

<details>
<summary>Planned file structure (work in progress)</summary>

```text
data_engineering_guide/
├─ README.md
│
├─ data-engineering-for-aws/
├─ 00_intro/
│  ├─ 00_how_to_use_this_repo.md
│  ├─ 01_learning_path.md
│  ├─ 02_glossary.md
│  └─ 03_interview_mode.md
│
├─ 01_foundations_on_aws/
│  ├─ 01_data_engineering_role_and_personas.md
│  ├─ 02_data_types_and_data_lifecycle.md
│  ├─ 03_data_architectures_overview.md
│  ├─ 04_sql_basics_for_de.md
│  ├─ 05_etl_vs_elt.md
│  ├─ 06_batch_vs_streaming.md
│  └─ 07_cost_basics_and_well_architected.md
│
├─ 02_storage_and_data_lakes/
│  ├─ 01_s3_fundamentals.md
│  ├─ 02_s3_security_encryption_versioning.md
│  ├─ 03_s3_partitioning_layout_conventions.md
│  ├─ 04_data_formats_parquet_orc_avro_json.md
│  ├─ 05_table_formats_delta_iceberg_hudi.md
│  └─ 06_data_lake_patterns_raw_curated_serving.md
│
├─ 03_databases_for_data_engineers/
│  ├─ 01_rds_and_aurora_for_analytics.md
│  ├─ 02_dynamodb_design_patterns.md
│  ├─ 03_redshift_fundamentals.md
│  ├─ 04_redshift_performance_dist_sort_wlm.md
│  ├─ 05_redshift_spectrum.md
│  └─ 06_transactionality_acid_isolation_in_aws.md
│
├─ 04_compute_and_execution/
│  ├─ 01_ec2_for_data_workloads.md
│  ├─ 02_lambda_for_data_engineering.md
│  ├─ 03_containers_ecs_eks_fargate.md
│  ├─ 04_emr_basics.md
│  ├─ 05_emr_spark_tuning.md
│  └─ 06_serverless_options_emr_serverless_glue_serverless.md
│
├─ 05_ingestion_and_movement/
│  ├─ 01_ingestion_strategies_and_tradeoffs.md
│  ├─ 02_dms_cdc.md
│  ├─ 03_datasync_and_transfer_family.md
│  ├─ 04_kinesis_data_streams.md
│  ├─ 05_kinesis_firehose.md
│  ├─ 06_msk_kafka_basics.md
│  ├─ 07_sqs_sns_eventbridge.md
│  └─ 08_dlq_and_replay_strategies.md
│
├─ 06_transformation_and_etl_glue/
│  ├─ 01_glue_catalog_and_crawlers.md
│  ├─ 02_glue_connections_jdbc_vpc.md
│  ├─ 03_glue_jobs_pyspark_core.md
│  ├─ 04_glue_bookmarks_incremental_loads.md
│  ├─ 05_glue_optimizations_dpus_partitions_shuffle.md
│  ├─ 06_data_quality_in_etl.md
│  └─ 07_schema_evolution_and_contracts.md
│
├─ 07_query_and_analytics/
│  ├─ 01_athena_fundamentals.md
│  ├─ 02_athena_optimization_partitions_ctas.md
│  ├─ 03_trino_presto_concepts.md
│  ├─ 04_lakehouse_patterns.md
│  └─ 05_bi_access_patterns.md
│
├─ 08_orchestration_and_automation/
│  ├─ 01_workflow_orchestration_principles.md
│  ├─ 02_step_functions_basics.md
│  ├─ 03_event_driven_orchestration.md
│  ├─ 04_airflow_mwaa_basics.md
│  ├─ 05_idempotency_retries_backfills.md
│  └─ 06_slas_alerting_runbooks.md
│
├─ 09_security_governance_and_compliance/
│  ├─ 01_iam_for_data_engineering.md
│  ├─ 02_vpc_endpoints_private_access.md
│  ├─ 03_kms_and_key_management.md
│  ├─ 04_lake_formation_governance.md
│  ├─ 05_pii_privacy_masking.md
│  └─ 06_audit_cloudtrail_config.md
│
├─ 10_observability_and_dataops/
│  ├─ 01_cloudwatch_logs_metrics_alarms.md
│  ├─ 02_monitoring_pipelines_and_cost.md
│  ├─ 03_data_lineage_and_catalog.md
│  ├─ 04_data_quality_frameworks.md
│  ├─ 05_incidents_postmortems.md
│  └─ 06_ci_cd_for_data_platforms.md
│
├─ 11_reference_architectures/
│  ├─ 01_batch_reference_architecture.md
│  ├─ 02_streaming_reference_architecture.md
│  ├─ 03_cdc_reference_architecture.md
│  ├─ 04_lakehouse_reference_architecture.md
│  └─ 05_cost_latency_reliability_tradeoffs.md
│
├─ 12_capstone_projects/
│  ├─ 01_batch_etl_lakehouse/
│  │  ├─ README.md
│  │  ├─ architecture.md
│  │  ├─ infra/
│  │  ├─ src/
│  │  └─ tests/
│  ├─ 02_streaming_real_time_pipeline/
│  │  ├─ README.md
│  │  ├─ architecture.md
│  │  ├─ infra/
│  │  ├─ src/
│  │  └─ tests/
│  └─ 03_cdc_to_lake_and_warehouse/
│     ├─ README.md
│     ├─ architecture.md
│     ├─ infra/
│     ├─ src/
│     └─ tests/
│
├─ 13_exam_and_interview_prep/
│  ├─ 01_aws_data_engineer_associate_topics.md
│  ├─ 02_questions_sql.md
│  ├─ 03_questions_aws_services.md
│  ├─ 04_questions_glue_athena_redshift.md
│  ├─ 05_system_design_questions.md
│  └─ 06_cheat_sheets_one_pagers.md
│
├─ assets/
│  ├─ diagrams/
│  └─ images/
│
└─ templates/
   ├─ topic_template.md
   ├─ aws_service_template.md
   ├─ reference_architecture_template.md
   └─ interview_answer_template.md

│
└─ data-engineering-for-blockchain/         # (new parallel track)
   ├─ README.md
   ├─ 00_intro/
   │  ├─ 00_how_to_use_this_track.md
   │  ├─ 01_blockchain_data_landscape.md
   │  ├─ 02_roles_and_systems_overview.md
   │  └─ 03_glossary_blockchain.md
   │
   ├─ 01_foundations/
   │  ├─ 01_blockchain_basics_for_de.md
   │  ├─ 02_crypto_economics_primitives.md
   │  ├─ 03_accounts_vs_utxo.md
   │  ├─ 04_transactions_blocks_finality_reorgs.md
   │  ├─ 05_event_logs_and_state.md
   │  ├─ 06_rpc_nodes_archives_indexers.md
   │  └─ 07_data_quality_challenges_in_chain_data.md
   │
   ├─ 02_sources_and_access/
   │  ├─ 01_rpc_fundamentals_json_rpc.md
   │  ├─ 02_web3_libraries_ethers_web3_py.md
   │  ├─ 03_archival_nodes_vs_full_nodes.md
   │  ├─ 04_public_providers_infura_alchemy_quicknode.md
   │  ├─ 05_rate_limits_retries_backoff.md
   │  ├─ 06_common_endpoints_blocks_txs_logs_traces.md
   │  └─ 07_security_api_keys_private_infra.md
   │
   ├─ 03_ingestion/
   │  ├─ 01_ingestion_patterns_batch_vs_stream.md
   │  ├─ 02_block_and_tx_ingestion.md
   │  ├─ 03_logs_events_ingestion.md
   │  ├─ 04_traces_debug_trace_transaction.md
   │  ├─ 05_mempool_and_pending_txs.md
   │  ├─ 06_reorg_handling_and_finality_windows.md
   │  ├─ 07_incremental_loads_watermarks.md
   │  └─ 08_dedup_idempotency_replay.md
   │
   ├─ 04_decoding_and_enrichment/
   │  ├─ 01_abi_decoding_events_calls.md
   │  ├─ 02_contract_metadata_sources.md
   │  ├─ 03_token_standards_erc20_erc721_erc1155.md
   │  ├─ 04_nft_metadata_ipfs_arweave.md
   │  ├─ 05_address_labeling_ens_entities.md
   │  ├─ 06_price_enrichment_ohlc_oracles_cex.md
   │  ├─ 07_protocol_specific_models_uniswap_aave_lido.md
   │  └─ 08_cross_chain_identity_bridges.md
   │
   ├─ 05_processing_frameworks/
   │  ├─ 01_pyspark_for_chain_data.md
   │  ├─ 02_sql_engines_trino_athena_clickhouse.md
   │  ├─ 03_polars_duckdb_for_local_analytics.md
   │  ├─ 04_stream_processing_flink_kafka_streams.md
   │  ├─ 05_graph_processing_networkx_neo4j.md
   │  └─ 06_ml_feature_engineering_on_chain.md
   │
   ├─ 06_storage_and_models/
   │  ├─ 01_lakehouse_for_blockchain.md
   │  ├─ 02_partitioning_by_chain_block_date.md
   │  ├─ 03_table_formats_delta_iceberg_hudi.md
   │  ├─ 04_raw_normalized_curated_serving_layers.md
   │  ├─ 05_entity_graph_modeling.md
   │  ├─ 06_address_balances_positions_pnl.md
   │  └─ 07_timeseries_modeling_prices_liquidity.md
   │
   ├─ 07_analytics_and_metrics/
   │  ├─ 01_defi_metrics_tvl_apr_fees.md
   │  ├─ 02_dex_metrics_volume_slippage_liquidity.md
   │  ├─ 03_tokenomics_inflation_unlocks_emissions.md
   │  ├─ 04_whales_wallet_clustering.md
   │  ├─ 05_user_cohorts_retention_funnel.md
   │  ├─ 06_risk_metrics_liquidations_bad_debt.md
   │  ├─ 07_alpha_signals_on_chain.md
   │  └─ 08_dashboards_and_semantic_layer.md
   │
   ├─ 08_governance_security_compliance/
   │  ├─ 01_data_integrity_verification_hashes.md
   │  ├─ 02_private_keys_secrets.md
   │  ├─ 03_pii_and_privacy_in_crypto.md
   │  ├─ 04_sanctions_kyc_aml_basics.md
   │  ├─ 05_mev_sandwich_and_attack_taxonomy.md
   │  └─ 06_auditability_and_reproducibility.md
   │
   ├─ 09_observability_and_dataops/
   │  ├─ 01_pipeline_monitoring_rpc_health.md
   │  ├─ 02_reorg_alerting_and_data_drift.md
   │  ├─ 03_data_quality_tests_chain_consistency.md
   │  ├─ 04_cost_management_rpc_storage_compute.md
   │  ├─ 05_ci_cd_for_indexers.md
   │  └─ 06_incident_response_playbooks.md
   │
   ├─ 10_reference_architectures/
   │  ├─ 01_blockchain_indexer_architecture.md
   │  ├─ 02_defi_analytics_platform_architecture.md
   │  ├─ 03_realtime_signals_architecture.md
   │  └─ 04_cross_chain_data_platform.md
   │
   ├─ 11_capstone_projects/
   │  ├─ 01_build_an_evm_indexer/
   │  │  ├─ README.md
   │  │  ├─ architecture.md
   │  │  ├─ infra/
   │  │  ├─ src/
   │  │  └─ tests/
   │  ├─ 02_defi_analytics_lakehouse/
   │  │  ├─ README.md
   │  │  ├─ architecture.md
   │  │  ├─ infra/
   │  │  ├─ src/
   │  │  └─ tests/
   │  └─ 03_realtime_mev_monitor/
   │     ├─ README.md
   │     ├─ architecture.md
   │     ├─ infra/
   │     ├─ src/
   │     └─ tests/
   │
   ├─ 12_interview_prep/
   │  ├─ 01_blockchain_data_engineer_questions.md
   │  ├─ 02_sql_for_chain_data_questions.md
   │  ├─ 03_system_design_web3_indexer.md
   │  ├─ 04_case_studies_protocol_analytics.md
   │  └─ 05_cheat_sheets.md
   │
   ├─ assets/
   │  ├─ diagrams/
   │  └─ images/
   │
   └─ templates/
      ├─ topic_template.md
      ├─ protocol_model_template.md
      ├─ indexer_design_template.md
      └─ interview_answer_template.md

```
</details>

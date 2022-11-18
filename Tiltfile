SOURCE_IMAGE = os.getenv("SOURCE_IMAGE", default='harbor.dieunkrauts.de/tap-wkld/todo-webapi-app-tap-test-dev') # update registry
LOCAL_PATH = os.getenv("LOCAL_PATH", default='.')
NAMESPACE = os.getenv("NAMESPACE", default='tap-test-dev')
OUTPUT_TO_NULL_COMMAND = os.getenv("OUTPUT_TO_NULL_COMMAND", default=' > /dev/null ')
NAME = "todo-webapi-app"


update_settings ( max_parallel_updates = 3 , k8s_upsert_timeout_secs = 600 , suppress_unused_image_warnings = None ) 

k8s_custom_deploy(
  NAME,
  apply_cmd="tanzu apps workload apply -f config/workload.yaml --debug --live-update" +
            " --local-path " + LOCAL_PATH +
            " --source-image " + SOURCE_IMAGE +
            " --namespace " + NAMESPACE +
            " --yes " +
            OUTPUT_TO_NULL_COMMAND +
            " && kubectl get workload " + NAME + " --namespace " + NAMESPACE + " -o yaml",
  delete_cmd="tanzu apps workload delete " + NAME + " --namespace " + NAMESPACE + " --yes",
  deps=['./bin'],
  container_selector='workload',
  live_update=[
    sync('./bin', '/workspace')
  ]
)

k8s_resource(NAME, port_forwards=["8080:8080"],
            extra_pod_selectors=[{'serving.knative.dev/service': 'todo-webapi-app'}])

allow_k8s_contexts('test-cluster') # update the context

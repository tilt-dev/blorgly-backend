def blorgly_backend_local():
  return blorgly_backend('devel')

def blorgly_backend_prod():
  return blorgly_backend('prod')

def blorgly_backend(env):
  image = build_docker_image('Dockerfile.base', 'gcr.io/blorg-dev/blorgly-backend:' + env + '-' + local('whoami').rstrip('\n'))
  image.add_mount('/blorgly-backend', git_repo('.'))
  image.add_cmd('echo hi')
  # print(image)
  yaml = local('python populate_config_template.py ' + env + ' 1>&2 && cat k8s-conf.generated.yaml')

  # this api might be cleaner than stderr stuff above
  # run('python populate_config_template.py ' + env')
  # yaml = read('k8s-conf.generated.yaml')

  # print(yaml)
  return k8s_service(yaml, image)

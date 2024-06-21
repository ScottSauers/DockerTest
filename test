import os
import subprocess

nextflow_content = f"""
process testDocker {{
    container '{os.environ['ARTIFACT_REGISTRY_DOCKER_REPO']}/ubuntu:latest'
    publishDir 'results', mode: 'copy'

    output:
    path 'output.txt'

    script:
    '''
    echo "Hello from Docker container!" > output.txt
    echo "Current date: $(date)" >> output.txt
    echo "Container OS: $(uname -a)" >> output.txt
    '''
}}

workflow {{
    testDocker()
    testDocker.out.view {{ it.text }}
}}
"""

with open("test_workflow.nf", "w") as f:
    f.write(nextflow_content)

print("Nextflow script created. Attempting to run workflow using Nextflow...")

nextflow_command = "nextflow run test_workflow.nf"

try:
    result = subprocess.run(nextflow_command, shell=True, check=True, capture_output=True, text=True)
    print("Nextflow output:")
    print(result.stdout)
except subprocess.CalledProcessError as e:
    print("Error running Nextflow:")
    print(e.stderr)

# Attempt to display the output file
try:
    with open("results/output.txt", "r") as f:
        print("\nContent of results/output.txt:")
        print(f.read())
except FileNotFoundError:
    print("\nOutput file not found in the results directory.")

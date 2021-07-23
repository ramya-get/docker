provider "aws" {
  region     = "ap-south-1"
}
resource "aws_vpc" "vpc" {
  cidr_block       = "10.0.0.0/16"
  instance_tenancy = "default"

  tags = {
    Name = "demovpc2"
  }
}
resource "aws_subnet" "Pub" {
  vpc_id     = aws_vpc.vpc.id
  cidr_block = "10.0.1.0/24"

  tags = {
    Name = "Public"
  }
}

resource "aws_internet_gateway" "igw" {
  vpc_id = aws_vpc.vpc.id

  tags = {
    Name = "IGW"
  }
}
resource "aws_eip" "ip" {
    vpc  = true
}

resource "aws_route_table" "rt1" {
  vpc_id = aws_vpc.vpc.id

  route  {
    cidr_block = "0.0.0.0/0"
    gateway_id = aws_internet_gateway.igw.id
  }
  tags = {
      name = "custom"
  }
}

resource "aws_route_table_association" "as_1" {
  subnet_id      = aws_subnet.Pub.id
  route_table_id = aws_route_table.rt1.id
}
resource "aws_security_group" "sg" {
  name        = "first-sg"
  description = "Allow TLS inbound traffic"
  vpc_id      = aws_vpc.vpc.id

  ingress {
    description      = "TLS from VPC"
    from_port        = 22
    to_port          = 22
    protocol         = "tcp"
    cidr_blocks      = [aws_vpc.vpc.cidr_block]
 }
  ingress {
    from_port        = 0
    to_port          = 0
    protocol         = "-1"
    cidr_blocks      = ["0.0.0.0/0"]
  }

  egress {
    from_port        = 0
    to_port          = 0
    protocol         = "-1"
    cidr_blocks      = ["0.0.0.0/0"]
  }
  
  tags = {
    Name = "fisrst-sg"
  }
}
resource "aws_instance" "ubuntu" {
  ami           = "ami-0c1a7f89451184c8b"
  instance_type = "t2.micro"
  associate_public_ip_address = true
   user_data = ${file(ansible.sh)}
  subnet_id = aws_subnet.Pub.id
  vpc_security_group_ids = [aws_security_group.sg.id]
  key_name = "ramyakey"

  tags = {
     Name = "Dev"
  }
}
resource "aws_instance" "ubuntu1" {
  ami           = "ami-0c1a7f89451184c8b"
  instance_type = "t2.micro"
  associate_public_ip_address = true
  subnet_id = aws_subnet.Pub.id
  vpc_security_group_ids = [aws_security_group.sg.id]
  key_name = "ramyakey"

  tags = {
    Name = "jenkins"
 }
}
resource "aws_instance" "ubuntu2" {
  ami           = "ami-0c1a7f89451184c8b"
  instance_type = "t2.micro"
  associate_public_ip_address = true
  subnet_id = aws_subnet.Pub.id
  vpc_security_group_ids = [aws_security_group.sg.id]
  key_name = "ramyakey"

  tags = {
    Name = "Docker"
  }
}
resource "aws_instance" "ubuntu3" {
  ami           = "ami-0c1a7f89451184c8b"
  instance_type = "t3.small"
  associate_public_ip_address = true
  subnet_id = aws_subnet.Pub.id
  vpc_security_group_ids = [aws_security_group.sg.id]
  key_name = "ramyakey"

  tags = {
    Name = "k8master"
  }
}
resource "aws_instance" "ubuntu4" {
  ami           = "ami-0c1a7f89451184c8b"
  instance_type = "t3.small"
  associate_public_ip_address = true
  subnet_id = aws_subnet.Pub.id
  vpc_security_group_ids = [aws_security_group.sg.id]
  key_name = "ramyakey"

  tags = {
    Name = "k8worker"
  }
}
